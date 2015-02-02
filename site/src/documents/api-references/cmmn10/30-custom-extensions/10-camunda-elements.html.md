---

title: 'Camunda Extension Elements'
category: 'Custom Extensions'

---

The following elements are extension elements for the `camunda` namespace `http://camunda.org/schema/1.0/cmmn`.

## camunda:caseExecutionListener

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      Add an execution listener to an event in the lifecycle of a case item.
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td><code>event</code></td>
    <td>
      The type of the lifecycle event for which the listener is called
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>class</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundaclass">camunda:class</a>
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>expression</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundaexpression">camunda:expression</a>
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>delegateExpression</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundadelegateexpression">camunda:delegateExpression</a>
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      The <code>event</code> attribute is required and must be one of the lifecycle events that are valid for the plan item it is defined on. For regular tasks, these are: <code>create</code>, <code>enable</code>, <code>disable</code>, <code>reenable</code>, <code>start</code>, <code>manualStart</code>, <code>complete</code>, <code>reactivate</code>, <code>terminate</code>, <code>exit</code, <code>parentTerminate</code>, <code>suspend</code>, <code>resume</code>, <code>parentSuspend</code>, <code>parentSuspend</code>, <code>close</code>, <code>occur</code>
    </td>
  </tr>
  <tr>
    <td></td>
    <td colspan="2">
      One of the attributes <code>class</code>, <code>expression</code> or <code>delegateExpression</code> is mandatory
    </td>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      Case,
      <a href="ref:#tasks-human-task">Human Task</a>,
      <a href="ref:#tasks-process-task">Process Task</a>,
      <a href="ref:#tasks-case-task">Case Task</a>,
      <a href="ref:#grouping-tasks-stage">Stage</a>,
      <a href="ref:#milestones-milestone">Milestone</a>,
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      <a href="ref:#custom-extensions-camunda-extension-elements-camundafield">camunda:field</a>,
      <a href="ref:#custom-extensions-camunda-extension-elements-camundascript">camunda:script</a>
    </td>
  </tr>
</table>

## camunda:expression

<table class="table table-striped">
<tr>
  <th>Description</th>
  <td colspan="2">
    Defines an expression to inject in delegated classes (see <a href="/guides/user-guide/#process-engine-delegation-code-field-injection">Field Injection</a>).
  </td>
</tr>
<tr>
  <th>Attributes</th>
  <td colspan="2">
    &ndash;
  </td>
</tr>
<tr>
  <th>Text Content</th>
  <td colspan="2">
    The expression to inject
  </td>
</tr>
<tr>
  <th>Constraints</th>
  <td colspan="2">
    &ndash;
  </td>
</tr>
<tr>
  <th>Parent elements</th>
  <td colspan="2">
    <a href="ref:#custom-extensions-camunda-extension-elements-camundafield">camunda:field</a>
  </td>
</tr>
<tr>
  <th>Child elements</th>
  <td colspan="2">
    &ndash;
  </td>
</tr>
</table>

## camunda:field

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      Defines the value to inject in delegated classes (see <a href="/guides/user-guide/#process-engine-delegation-code-field-injection">Field Injection</a>).
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td><code>name</code></td>
    <td>
      The name of the field
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>expression</code></td>
    <td>The value of the field as expression</td>
  </tr>
  <tr>
    <td></td>
    <td><code>stringValue</code></td>
    <td>The value of the field as String</td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      Only one attribute of <code>stringValue</code> and <code>expression</code> or one of the child elements <code>string</code> and <code>expression</code> can be used
    </td>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      <a href="ref:#custom-extensions-camunda-extension-elements-camundavariablelistener">camunda:variableListener</a>,
      camunda:caseExecutionListener
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      <a href="ref:#custom-extensions-camunda-extension-elements-camundaexpression">camunda:expression</a>,
      <a href="ref:#custom-extensions-camunda-extension-elements-camundastring">camunda:string</a>
    </td>
  </tr>
</table>

## camunda:in

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      This element specifies variables which should be passed to the sub instance (process instance or case instance), see <a href="ref:#tasks-process-task-exchange-variables">passing variables to called process</a> or <a href="ref:#tasks-case-task-exchange-variables">passing variables to called case</a> for more information.
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td><code>source</code></td>
    <td>
      A name of a variable to be passed in
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>sourceExpression</code></td>
    <td>
      An expression to be evaluated and passed in
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>variables</code></td>
    <td>
      Can be set to <code>all</code> to pass all variables in
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>target</code></td>
    <td>
      Name of the variable inside the sub instance
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>businessKey</code></td>
    <td>
      Set the business key of the sub instance
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      Only one of the attributes <code>source</code>, <code>sourceExpression</code>, <code>variables</code> or <code>businessKey</code> can be used
    </td>
  </tr>
  <tr>
    <td></td>
    <td colspan="2">
      The attribute <code>target</code> is required if the <code>source</code> or <code>sourceExpression</code> attribute is used
    </td>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      <a href="ref:#tasks-process-task">Process Task</a>,
      <a href="ref:#tasks-case-task">Case Task</a>
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      &ndash;
    </td>
  </tr>
</table>


## camunda:out

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      This element specifies variables which should be passed back from the sub instance (process instance or case instance), see <a href="ref:#tasks-process-task-exchange-variables">passing variables to called process</a> or <a href="ref:#tasks-case-task-exchange-variables">passing variables to called case</a> for more information.
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td><code>source</code></td>
    <td>
      A name of a variable to be passed back
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>sourceExpression</code></td>
    <td>
      An expression to be evaluated and passed back
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>variables</code></td>
    <td>
      Can be set to <code>all</code> to pass all sub instance variables back
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>target</code></td>
    <td>
      Name of the variable inside the calling case instance
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      Only one of the attributes <code>source</code>, <code>sourceExpression</code>, <code>variables</code> or <code>businessKey</code> can be used
    </td>
  </tr>
  <tr>
    <td></td>
    <td colspan="2">
      The attribute <code>target</code> is required if the <code>source</code> or <code>sourceExpression</code> attribute is used
    </td>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      <a href="ref:#tasks-process-task">Process Task</a>,
      <a href="ref:#tasks-case-task">Case Task</a>
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      &ndash;
    </td>
  </tr>
</table>

## camunda:script

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      A script element. The script is executed and the return value is used as mapping value.
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td><code>scriptFormat</code></td>
    <td>
      The format identifier, normally the language of the script source code
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>resource</code></td>
    <td>
      equivalent to <a href="ref:#custom-extensions-camunda-extension-attributes-camundaresource">
      camunda:resource</a>
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      The <code>scriptFormat</code> attribute is required
    </td>
  </tr>
    <td></td>
    <td colspan="2">
      If the <code>resource</code> attribute is used, no source code text content is allowed
    </td>
  <tr>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      <a href="ref:#custom-extensions-camunda-extension-elements-camundavariablelistener">
        camunda:variableListener</a>,
      camunda:caseExecutionListener
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      &ndash;
    </td>
  </tr>
</table>


## camunda:string

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      Defines a String value to inject in delegated classes (see <a href="/guides/user-guide/#process-engine-delegation-code-field-injection">Field Injection</a>).
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td colspan="2">
      &ndash;
    </td>
  </tr>
  <tr>
    <th>Text Content</th>
    <td colspan="2">
      The String value to inject
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      &ndash;
    </td>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      <a href="ref:#custom-extensions-camunda-extension-elements-camundafield">camunda:field</a>
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      &ndash;
    </td>
  </tr>
</table>

## camunda:taskListener

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      Add a task listener to a task event.
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td><code>event</code></td>
    <td>
      The type of the event for which the listener is called
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>class</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundaclass">camunda:class</a>
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>expression</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundaexpression">camunda:expression</a>
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>delegateExpression</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundadelegateexpression">camunda:delegateExpression</a>
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      The <code>event</code> attribute is required and must be one of the task events: <code>create</code>, <code>assignment</code>, <code>complete</code> or <code>delete</code>
    </td>
  </tr>
  <tr>
    <td></td>
    <td colspan="2">
      One of the attributes <code>class</code>, <code>expression</code> or <code>delegateExpression</code> is mandatory
    </td>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      <a href="ref:#tasks-human-task">Human Task</a>
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      <a href="ref:#custom-extensions-camunda-extension-elements-camundafield">camunda:field</a>,
      <a href="ref:#custom-extensions-camunda-extension-elements-camundascript">camunda:script</a>
    </td>
  </tr>
</table>

## camunda:variableListener

<table class="table table-striped">
  <tr>
    <th>Description</th>
    <td colspan="2">
      Adds custom code to listen to variable events such as the creation, update or deletion of a variable. A listener defined on a model scope (like the case plan model, a human task, etc.) is notified for all variable events that occur in this scope or any of its subordinate scopes.
    </td>
  </tr>
  <tr>
    <th>Attributes</th>
    <td><code>event</code></td>
    <td>
      The type of the event for which the listener is called
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>class</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundaclass">camunda:class</a>
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>expression</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundaexpression">camunda:expression</a>
    </td>
  </tr>
  <tr>
    <td></td>
    <td><code>delegateExpression</code></td>
    <td>
      see <a href="ref:#custom-extensions-camunda-extension-attributes-camundadelegateexpression">camunda:delegateExpression</a>
    </td>
  </tr>
  <tr>
    <th>Constraints</th>
    <td colspan="2">
      Valid values for the <code>event</code> attribute are <code>create</code>, <code>update</code> and <code>delete</code>. This attribute is optional and if left out, the listener is notified for all kinds of events.
    </td>
  </tr>
  <tr>
    <td></td>
    <td colspan="2">
      Either one of the attributes <code>class</code>, <code>expression</code> or <code>delegateExpression</code>, or a <code>camunda:script</code> child element is mandatory.
    </td>
  </tr>
  <tr>
    <th>Parent elements</th>
    <td colspan="2">
      Case Plan Model,
      <a href="ref:#grouping-tasks-stage">Stage</a>,
      <a href="ref:#tasks-human-task">Human Task</a>,
      <a href="ref:#tasks-process-task">Process Task</a>,
      <a href="ref:#tasks-case-task">Case Task</a>
    </td>
  </tr>
  <tr>
    <th>Child elements</th>
    <td colspan="2">
      <a href="ref:#custom-extensions-camunda-extension-elements-camundafield">camunda:field</a>,
      <a href="ref:#custom-extensions-camunda-extension-elements-camundascript">camunda:script</a>
    </td>
  </tr>
</table>


