---

title: 'Multi-Tenancy'
category: 'Process Engine'

---

*Multi-tenancy* regards the case in which a single Camunda installation should serve more than one tenant. For each tenant, certain guarantees of isolation should be made. For example, one tenant's process instances should not interfere with those of another tenant.

Multi-tenancy can be achieved on different levels of data isolation. On the one end of the spectrum, different tenants' data can be stored in [different databases](ref:#process-engine-multi-tenancy-one-process-engine-per-tenant) by configuring multiple process engines, while on the other end of the spectrum, runtime entities can be associated with [tenant markers](ref:#process-engine-multi-tenancy-a-tenant-marker-per-process-instance) and are stored in the same tables. In between these two extremes, it is possible to separate tenant data into different schemas or tables.

<div class="alert alert-info">
  <p>
    <strong>Recommended Approach:</strong>
    <p>We recommend the approach of multiple process engines (i.e., isolation into different databases/schemas/tables) over the tenant marker approach as it is more robust and easier to use.</p>
  </p>
</div>

### One Process Engine Per Tenant

Database-, schema-, and table-based multi-tenancy can be enabled by configuring one process engine per tenant. Each process engine can be configured to point to a different portion of the database. While they are isolated in that sense, they may all share computational resources such as a data source (when isolating via schemas or tables) or a thread pool for asynchronous job execution. Furthermore, the Camunda API offers convenient access to different process engines based on a tenant identifier.

#### Data isolation

Database, schema or table level

#### Advantages

* Strict data separation
* Hardly any performance overhead for application servers due to resource sharing
* In case one tenant's database state is inconsistent, no other tenant is affected
* Camunda Cockpit, Tasklist, and Admin offer tenant-specific views out of the box by [switching between different process engines](ref:#cockpit-dashboard-multi-tenancy)

#### Disadvantages

* Additional process engine configuration necessary
* No out-of-the-box support for tenant-independent queries

#### Implementation

Working with different process engines for multiple tenants comprises the following steps:

* **Configuration** of process engines
* **Deployment** of process definitions for different tenants to their respective engines
* **Access** to a process engine based on a tenant identifier via the Camunda API


<div class="alert alert-info">
  <p>
    <strong>Tutorial</strong>
    <p>You can find a tutorial <a href="ref:/real-life/how-to/#process-engine-multi-tenancy">here</a> that shows how to implement multi-tenancy with data isolation by schemas.</p>
  </p>
</div>

##### Configuration

Multiple process engines can be configured in a configuration file or via Java API. Each engine should be given a name that is related to a tenant such that it can be identified based on the tenant. For example, each engine can be named after the tenant it serves. See the [Process Engine Bootstrapping](ref:#process-engine-process-engine-bootstrapping) section for details.

The process engine configuration can be adapted to achieve either database-, schema- or table-based isolation of data. If different tenants should work on entirely different databases, they have to use different jdbc settings or different data sources. For schema- or table-based isolation, a single data source can be used which means that resources like a connection pool can be shared among multiple engines. The configuration option [databaseTablePrefix](ref:/api-references/deployment-descriptors/#tags-process-engine-configuration-configuration-properties) can be used to configure database access in this case.

For background execution of processes and tasks, the process engine has a component called [job executor](ref:#process-engine-the-job-executor). The job executor periodically acquires jobs from the database and submits them to a thread pool for execution. For all process applications on one server, one thread pool is used for job execution. Furthermore, it is possible to share the acquisition thread between multiple engines. This way, resources are still manageable even when a large number of process engines is used. See the section [The Job Executor and Multiple Process Engines](ref:#process-engine-the-job-executor-the-job-executor-and-multiple-process-engines) for details.

Multi-tenancy settings can be applied in the various ways of configuring a process engine. The following is an example of a [bpm-platform.xml](ref:#process-engine-process-engine-bootstrapping-configure-process-engine-in-bpm-platformxml) file that specifies engines for two tenants that share the same database but work on different schemas:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<bpm-platform xmlns="http://www.camunda.org/schema/1.0/BpmPlatform"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://www.camunda.org/schema/1.0/BpmPlatform http://www.camunda.org/schema/1.0/BpmPlatform">

  <job-executor>
    <job-acquisition name="default" />
  </job-executor>

  <process-engine name="tenant1">
    <job-acquisition>default</job-acquisition>
    <configuration>org.camunda.bpm.engine.impl.cfg.StandaloneProcessEngineConfiguration</configuration>
    <datasource>java:jdbc/ProcessEngine</datasource>

    <properties>
      <property name="databaseTablePrefix">TENANT_1.</property>

      <property name="history">full</property>
      <property name="databaseSchemaUpdate">true</property>
      <property name="authorizationEnabled">true</property>
    </properties>
  </process-engine>

  <process-engine name="tenant2">
    <job-acquisition>default</job-acquisition>
    <configuration>org.camunda.bpm.engine.impl.cfg.StandaloneProcessEngineConfiguration</configuration>
    <datasource>java:jdbc/ProcessEngine</datasource>

    <properties>
      <property name="databaseTablePrefix">TENANT_2.</property>

      <property name="history">full</property>
      <property name="databaseSchemaUpdate">true</property>
      <property name="authorizationEnabled">true</property>
    </properties>
  </process-engine>
</bpm-platform>
```

##### Deployment

When developing process applications, i.e., process definitions and supplementary code, some processes may be deployed to every tenant's engine while others are tenant-specific. The processes.xml deployment descriptor that is part of every process application offers this kind of flexibility by the concept of *process archives*. One application can contain any number of process archive deployments, each of which can be deployed to a different process engine with different resources. See the section on the [processes.xml deployment descriptor](ref:#process-applications-the-processesxml-deployment-descriptor) for details.

The following is an example that deploys different process definitions for two tenants. It uses the configuration property `resourceRootPath` that specifies a path in the deployment that contains process definitions to deploy. Accordingly, all the processes under `processes/tenant1` on the application's classpath are deployed to engine `tenant1`, while all the processes under `processes/tenant2` are deployed to engine `tenant2`.

```
<process-application
  xmlns="http://www.camunda.org/schema/1.0/ProcessApplication"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <process-archive name="tenant1-archive">
    <process-engine>tenant1</process-engine>
    <properties>
      <property name="resourceRootPath">classpath:processes/tenant1/</property>

      <property name="isDeleteUponUndeploy">false</property>
      <property name="isScanForProcessDefinitions">true</property>
    </properties>
  </process-archive>

  <process-archive name="tenant2-archive">
    <process-engine>tenant2</process-engine>
    <properties>
      <property name="resourceRootPath">classpath:processes/tenant2/</property>

      <property name="isDeleteUponUndeploy">false</property>
      <property name="isScanForProcessDefinitions">true</property>
    </properties>
  </process-archive>

</process-application>
```

##### Access

In order to access a specific tenant's process engine at runtime, it has to be identified by its name. The Camunda engine offers access to named engines in various programming models:

* **Plain Java API**: Via the [ProcessEngineService](ref:#runtime-container-integration-bpm-platform-services-processengineservice) any named engine can be accessed.
* **CDI Integration**: Named engine beans can be injected out of the box. The [built-in CDI bean producer](ref:#cdi-and-java-ee-integration-built-in-beans) can be specialized to access the engine of the current tenant dynamically.
* **Via JNDI on JBoss/Wildfly**: On JBoss and Wildfly, every container-managed process engine can be [looked up via JNDI](ref:#runtime-container-integration-the-camunda-jboss-subsystem-looking-up-a-process-engine-in-jndi).

### A Tenant Marker Per Process Instance

The least isolated approach is to add tenant-specific markers in form of a process variable to running processes. This marker identifies the tenant in which context the process instance is running. In order to access only data for a specific tenant, many process engine queries allow to filter by process variables. A calling application must make sure to filter according to the correct tenant.

#### Data isolation

Row level with applications responsible for filtering

#### Advantages

* Straightforward querying for data across multiple tenants as the data for all tenants is organized in the same tables.

#### Disadvantages

* Requires tenant-aware queries
* Querying with process variables may reduce performance.
* Risk of disclosing data that belong to other tenants because of bugs or careless application programming.

#### Implementation

Working with tenant markers comprises the following aspects:

* **Instantiating** tenant markers
* **Querying** for process entities of different tenants

##### Instantiating

A tenant marker can be added to a process instance by passing it as a process variable on instantiation:

```java
Map<String, Object> variables = new HashMap<String, Object>();
variables.put("TENANT_ID", "tenant1");

runtimeService.startProcessInstanceByKey("some process", variables);
```

For process definitions that are specific to a single tenant, it is also possible to use an [execution listener](ref:#process-engine-delegation-code-execution-listener) on the start event that immediately sets the variable after instantiation.

##### Querying

Process applications that retrieve tenant-specific data must ensure that they filter by the tenant marker in order to isolate data between tenants. The following is a query that retrieves all process instances for tenant `tenant1`:

```java
List<ProcessInstance> processInstances =
  runtimeService.createProcessInstanceQuery()
    .variableValueEquals("TENANT_ID", "tenant1")
    .list();
```

Other queries like task and execution queries offer the same filtering capabilities. For correlation via the `RuntimeService#correlateMessage` methods, tenant-specific correlation can be achieved by adding the tenant marker as a correlation key like:

```java
runtimeService.createMessageCorrelation("someMessage")
  .processInstanceVariableEquals("TENANT_ID", "tenant1")
  .correlate();
```
