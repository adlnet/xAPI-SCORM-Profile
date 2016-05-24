Experience API SCORM Profile
==================
The Experience API SCORM Profile is a companion document to the Experience API Specification. Its goal is to provide guidance to those in the SCORM community looking for a way to leverage the Experience API. This profile offers guidelines for representing SCORM data and events as Experience API Statements. Use of this profile will provide consistency in reporting and retrieving data traditionally stored in an LMS, and allow for the development of interoperable tools outside of the typical LMS environment.

# Launching and Initializing Activities
In a SCORM environment, an activity is launched by the LMS. The LMS can provide activities with launch parameters and initial data model values. This helps a SCO to work correctly in the current environment. In comparison, xAPI activities can exist outside of an LMS - information such as learner name and identifier, or the location of the xAPI LRS may be unknown at the launch of the activity.

Solutions such as Rustici Software Launch, AICC CMI5 and IMS LTI all provide ways for systems, such as an LMS, to launch and initialize activities that are not managed by the LMS. Any of those solutions may be leveraged to solve specific, individual issues that are not addressed in this document. At a minimum the following properties are recommended for launching and initializing activities:  
- entry: ab-initio or resume
- endpoint: LRS endpoint 
- actor: Agent Account
  - Account Homepage: location of the LMS that holds the user account information
  - Account Name: an identifier for the learner that is at least unique to the scope of the LMS
    - It is recommended that the name is does not contain personally identifiable information such as the learner’s name
- courseiri: IRI that identifies the entire course within at least the context of the LMS
  - It is recommended that this identifier is unique and defined by a domain which your organization controls. ex: `http://adlnet.gov/courses/compsci/xxx`  

Since xAPI activites do not need to be web-based, the means of initializing the activities will vary. Following are three strategies:  

### Web-Based Activities
The activity is hosted on some web server and is launched in a web browser. This strategy is similar to SCORM content but is not required to be hosted by the LMS. In this scenario, the LMS, or some system responsible for knowing about the learner and this activity, launches the activity with the launch parameters.  
__Example:__
__Decoded for readability:__  
``` 
http://adlnet.gov/mycontent?entry=ab-initio&endpoint=https://lrs.adlnet.gov/xapi/&actor={"account":{"homePage":"http://lms.adlnet.gov/scorm/","name":"149893"}}&courseiri=http://adlnet.gov/courses/compsci/xxx 
```  
__URL-encoded:__  
``` 
http://adlnet.gov/mycontent?entry=ab-initio&endpoint=https%3A%2F%2Flrs.adlnet.gov%2Fxapi%2F&actor=%7B%22account%22%3A%7B%22homePage%22%3A%22http%3A%2F%2Flms.adlnet.gov%2Fscorm%2F%22%2C%22name%22%3A%22149893%22%7D%7D&courseiri=http%3A%2F%2Fadlnet.gov%2Fcourses%2Fcompsci%2Fxxx 
```  

### LMS-Provided Endpoint
The LMS provides an endpoint that activity providers can query to retrieve the launch parameters. Web-based applications can still access this information via an AJAX GET request to the LMS provided endpoint. All other content would access this information in a similar way by issuing an HTTP GET request for the launch parameters.  
__Request:__  
``` HTTP GET launch/?agentid=<learner id>&courseiri=<course iri> ```  
> NOTE: How the activity provider gets the learner id, course IRI and location of the endpoint is up to the content developer and the LMS.  
  
__Response:__  
``` javascript
Content-Type: application/json
{ 
   "entry":"ab-initio", 
   "endpoint":"https://lrs.adlnet.gov/xapi/", 
   "actor":
   {
      "account":
      {
          "homePage":"http://lms.adlnet.gov/scorm/",
          "name":"149893"
      }
   }, 
   "courseiri":"http://adlnet.gov/courses/compsci/xxx"
}
```  
> NOTE: The course IRI in this response shall be used in all statements issued for this activity. This requirement allows for the LMS to change the IRI from the one originally configured in the activity. This may be necessary to accommodate for changes due to various sessions, or other changes that occurred since the initial creation of the course IRI.  

### Out-of-Band Configuration
If the above launch options are not possible developers can preconfigure the activity provider or allow for user input to configure the launch parameters. Work with the LMS and LRS providers for configuration details.  

# Use
This document is intended to be used in addition to the [xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md). All communication, data formats and experiences shall follow the requirements in the xAPI specification. The guidance provided in this document is to add information specifically to support SCORM communications in xAPI format. 

### Supporting SCORM Features
SCORM is a mature and formal specification that allows for delivering content and tracking learner’s progress. By identifying the key features in SCORM, it is possible to provide guidance on how to implement the same features with the xAPI, such as:

- Launching and Initializing Content
- Supporting the SCORM Temporal Model
- Mapping the SCORM data model to xAPI Statements

### General Guidance on Statements
In the xAPI, activity providers and activities issue statements about the learner’s experiences. These statements hold the data about the experience, such as, if the learner was successful, or did the learner complete the content. Additional information and guidance is provided below. All statements issued shall meet the requirements in the xAPI specification and the guidance described in the following sections.  

#### Internationalized Resource Identifier (IRI)
[IRIs](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#def-iri) are used as identifiers in many parts of the xAPI specification. Their syntax consists of a hierarchical part, which is broken into an authority and a path. The authority part shall represent a domain of your organization - ie. adlnet.gov, army.mil. The path part shall identify the object - /courses/cs/CS101. If you are identifying something that cannot be represented by an fully qualified path, use a tag IRI - tag:adlnet.gov,2013:expapi:0.9:extensions - where the domain is a domain you control, the date is the date this tag was created, the next part is the type - scorm, xapi, the version - 2004, 1.2, 1.0.1, and the last part is the identifier. (see [http://www.ietf.org/rfc/rfc4151.txt](http://www.ietf.org/rfc/rfc4151.txt))  
  
__Good IRIs__  
Good IRIs uniquely identify an object  
  <pre>`http://adlnet.gov/activities/courses/CS/CS101`</pre>
  <pre>`http://mydomain.com/content/understanding-fire-safety`</pre>
__Bad IRIs__  
Bad IRIs are not unique and could conflict with others using the same identifier  
    <pre>`act:my-activity01`</pre>
    <pre>`http://example.com/activity/01`</pre>

#### Actor
The actor property refers to the learner that is interacting with the activity. The actor should be able to be mapped back to the learner in the LMS. Any of the [Actor formats described in the xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#actor) are allowed. However some formats can potentially expose personally identifiable information, it is recommended to use a format that protects the learner’s information.  
__Agent__
``` javascript
  "actor":{
    "account":{
       "homePage":"http://lms.adlnet.gov/",
       "name":"500-627-490"
    }
  }
``` 
__Group__
``` javascript
"actor":{
  "objectType":"Group",
  "name":"team a",
  "member":[
    {
       "account":{
          "homePage":"http://lms.adlnet.gov/",
          "name":"500-627-490"
       }
    },
    {
       "account":{
          "homePage":"http://lms.adlnet.gov/",
          "name":"500-344-153"
       }
    }
  ]
}
```  

#### Verb
Verbs in the xAPI spec represent the action that is being recorded for this experience, such as "read" a book and "answered" a question. The xAPI allows for any verb to be used within a statement assuming it follows the [rules defined in the xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#verb). However this flexibility causes issues when trying to report on the data. For consistency and interoperability, those implementing this profile shall use the verbs maintained by ADL at http://adlnet.gov/expapi/verbs/, and use the definition and usage on the ADL Verb pages as guidance.  
__Verb__  
``` javascript
"verb":{
    "id":"http://adlnet.gov/expapi/verbs/initialized",
    "display":{
       "en-US":"initialized"
    }
 }
```  
Some verbs have special meaning when related to SCORM. Implementers of this profile shall use these verbs only when reporting the events in the following table:  
<table>
  <tr><th>xAPI Verb</th><th>SCORM Equivalent</th><th>Description</th><th>Scope</th></tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/initialized">initialized</a></td>
    <td>Initialize()<br>LMSInitialize()</td>
    <td>Initialization of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/terminated">terminated</a></td>
    <td>Terminate()<br>LMSFinish()</td>
    <td>Termination of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/suspended">suspended</a></td>
    <td>cmi.exit=suspend<br>cmi.core.exit=suspend</td>
    <td>Suspension of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/resumed">resumed</a></td>
    <td>cmi.entry=resume<br>cmi.core.entry=resume</td>
    <td>Resumption of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/passed">passed</a></td>
    <td>cmi.success_status=passed<br>cmi.core.lesson_status=passed</td>
    <td>The leaner's performance on this attempt at least met the minimum threshold for the activity</td>
    <td>course<br>SCO<br>objective</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/failed">failed</a></td>
    <td>cmi.success_status=failed<br>cmi.core.lesson_status=failed</td>
    <td>The performance on this attempt did not meet the minimum threshold for the activity</td>
    <td>course<br>SCO<br>objective</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/scored">scored</a></td>
    <td>cmi.scored.scaled<br>cmi.core.score.raw</td>
    <td>Used when the activity wants to record a score without implying a level of satisfaction or completion of the activity, or wants to provide evidence toward satisfaction or completion</td>
    <td>course<br>SCO<br>objective</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/completed">completed</a></td>
    <td>cmi.completion_status=completed<br>cmi.core.lesson_status=completed</td>
    <td>The learner has experienced a sufficient amount of the activity</td>
    <td>course<br>SCO<br>objective</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/responded">responded</a></td>
    <td>cmi.interactions.n.learner_response<br>cmi.interactions.n.student_response</td>
    <td>The learner's response to some interaction</td>
    <td>interaction</td>
  </tr>
</table>

#### Object
The object property describes the item with which the learner is interacting. The type of object could be an Activity, an Actor or Group, or a Statement (as StatementRef or Sub-Statement). If it is an Actor or a Statement follow the rules defined in the [xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#414-object). However if it is an Activity, follow the rules in the xAPI specification and the rules outlined below.  

##### Activity
Things like courses, SCOs, and objectives are considered activities. Since these experiences are commonly what SCORM content wants to capture, the following sections detail the recommended usage to create interoperable statements.  

###### Activity IRI
Activity IDs are required to be [IRIs](http://en.wikipedia.org/wiki/Internationalized_resource_identifier) and follow the requirements within the [xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4141-when-the-objecttype-is-activity). In addition it is recommended that the IRIs are IRLs and that the location contains the metadata ([Activity Definition](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#activity-definition)) of the activity. Additionally activity IRIs should be constructed in a predictable way to make interpreting those IRIs possible in a standard way.  

###### Course IRI
The course IRI may be decided by the organization as long as it follows the IRI rules described in the xAPI spec. The activity provider or activity must support the launch courseIRI property, described in the launch section of this document, and update the course IRI to that property value if the local and launch-provided values differ. This allows for content to use the most up-to-date identifier in the context of the current launch.  
  
__Guidelines for Activity IRI Construction__  
- Follow the guidance described in the Internationalized Resource Identifier (IRI) section in this document
- Course IRIs should be in the format: `<scheme>://<authority>/<course path>`
  - Course path is any IRI path to the course activity definition, ie `courses/CS/101`
- SCO IRIs should be in the format: `<courseIRI>/<sco path>`
  - SCO path is any IRI path to the SCO activity definition, ie `lesson/01`
- Interaction IRIs should be in the format: `<scoIRI>/interactions/<interaction path>`
  - Interaction path is any IRI path to the interaction activity definition, ie `multi-choice/07`
- Objectives IRIs vary depending on the scope of the objective
  - If the objective is local to the SCO: `<scoIRI>/objectives/<objective path>`
  - If the objective is available to the entire course: `<courseIRI>/objectives/<objective path>`
  - If the objective is available to any course (global): `<scheme>://<authority>/objectives/<objective path>`
  - Objective path is any IRI path to the objective activity definition, ie `recognize-shapes-obj/`

###### Activity Definition
The [activity definition](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#activity-definition) provides information about the activity. At a minimum all activities should include an activity definition with a name, description and type. If the activity is representing a SCORM interaction, follow the rules outlined in the xAPI specification for [interaction activities](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#interactionacts).  
  
###### Activity Types  
Activity types are used to identify what an activity represents, both to humans and machines. The type could be used to reporting systems to determine, for example, what Statements are about assessments. Including an activity type is recommended. The following table describes types to be used for common SCORM concepts such as SCOs and interactions.  
  
<table>
<tr><th>SCORM object</th><th>Activity Type IRI</th></tr>
<tr><td>course</td><td><a href="http://adlnet.gov/expapi/activities/course">http://adlnet.gov/expapi/activities/course</a></td></tr>
<tr><td>module</td><td><a href="http://adlnet.gov/expapi/activities/module">http://adlnet.gov/expapi/activities/module</a></td></tr>
<tr><td>SCO</td><td><a href="http://adlnet.gov/expapi/activities/lesson">http://adlnet.gov/expapi/activities/lesson</a></td></tr>
<tr><td>assessment</td><td><a href="http://adlnet.gov/expapi/activities/assessment">http://adlnet.gov/expapi/activities/assessment</a></td></tr>
<tr><td>interaction</td><td><a href="http://adlnet.gov/expapi/activities/interaction">http://adlnet.gov/expapi/activities/interaction</a></td></tr>
<tr><td>objective</td><td><a href="http://adlnet.gov/expapi/activities/objective">http://adlnet.gov/expapi/activities/objective</a></td></tr>
<tr><td>attempt</td><td><a href="http://adlnet.gov/expapi/activities/attempt">http://adlnet.gov/expapi/activities/attempt</a></td></tr>
</table>

#### Result
The [result property of a statement](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#415-result) represents the outcome of a learner experience. Statements issued using a result shall follow the requirements listed in the xAPI specification and the additional guidance described in this document.

#### Context
The context property adds additional contextual information about the learner experience. Information like who is the instructor of this content, with what registration this experience is associated, or to what activities this experience is related. Statements shall meet all the [requirements in the xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#416-context) and the additional guidelines described in this document.  
  
##### Context Registration
[Context registration](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4161-registration-property) is used to store an LMS registration value. This is value is up to the organization, LMS or developer and provides a way to easily query the LRS for Statements related to the registration value. [See the xAPI spec for query options](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#723-getstatements).  

##### Context Activities
[Context activities](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4162-contextactivities-property) allow statements to be related to other activities. Regarding this profile, the context activities are used to relate Statements by SCO, course, assessment, or other groupings. The following rules are defined to support those groupings.  

<table>
<tr><th>Type</th><th>Use</th></tr>
<tr><td>parent</td><td>Used to identify the activity which contains the current activity, such as the activity of the SCO that contains an assessment, interaction, or objective.</td></tr>
<tr><td>grouping</td><td>Used to identify the course activity and any other activities that should be grouped together.</td></tr>
<tr><td>category</td><td>All Statements based on this profile shall include the xAPI SCORM Profile activity.<br> {"id":"https://w3id.org/xapi/adl/profiles/scorm"}</td></tr>
<tr><td>other</td><td>Up to the organization or developer.</td></tr>
</table>

#### Timestamp
The [timestamp property](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#417-timestamp) gives the activity provider or activity the ability to set the date and time when the experience occurred. This is different from the [stored timestamp](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#418-stored) property, which indicates when the LRS saved the statement about the experience. All statements shall set the timestamp property. By doing this, it makes it possible to order statements in the same order as they were submitted by the activity provider or activity.

#### Authority
The [authority property](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#419-authority) for each statement is set by the LRS to the agent who submitted the statement. This property can be used to filter statements based on the activity provider or activity. By using this property as a GET statements filter, it is possible to narrow down the returned statements to the ones submitted by agents whom your organization trusts.

#### Attachments
Attachments give the ability to include supporting documents or information that do not directly map to a section of an xAPI statement. Examples of attachments include PDF certificates, essays, videos, and the JSON web signature. Attachments are not returned by the LRS by default, but can be included by adding the attachments filter to GET statements requests. [See the xAPI specification for more details](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#signature).
