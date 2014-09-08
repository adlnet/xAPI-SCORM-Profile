# Experience API SCORM Profile
## Advanced Distributed Learning (ADL)

>"Copyright 2014 Advanced Distributed Learning (ADL) Initiative, U.S. Department of Defense

>Licensed under the Apache License, Version 2.0 (the "License"). You may not use this file except 
>in compliance with the License. You may obtain a copy of the License at
>http://www.apache.org/licenses/LICENSE-2.0

>Unless required by applicable law or agreed to in writing, software distributed under the License 
>is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express 
>or implied. See the License for the specific language governing permissions and limitations under 
>the License."

## 1.0 Purpose
The xAPI was created in response to the eLearning community’s desire to modernize the Sharable Content Object Reference Model (SCORM) capabilities, which was initially developed to make courseware interoperable with learning management systems. Since its introduction in 2000, SCORM has played a critical role in the proliferation of online training and education courses. The Experience API (xAPI) introduces a new paradigm for tracking and recording learning-related data. We can track learners as they perform work tasks, produce work outputs, communicate, collaborate, and engage in just about any other online activity. This API uses a flexible data format that supports many different use cases and needs. However with this flexibility arises the need for developers to formalize the data they track and what that data means. By formalizing the data reported by content and the format of that data, tools can be created that can make sense of that data.

The Sharable Content Object Reference Model (SCORM) community is one area that could benefit from providing a formalized method for compiling and formatting learner experiences. Using the xAPI and the guidance contained within this profile, developers can create xAPI statements that can be interpreted by any other system that understands the guidelines described in this document. This will support the integrity and consistency of the data across SCORM content and provide a base vocabulary needed for interpreting and reporting on that data.

## 2.0 When to Use this Profile
SCORM is well established and accepted in many organizations around the world as the interoperable way to deliver and track web-based learning content. If the traditional LMS and content model is working for your organization, there is no need to change to use the xAPI.

However there are use cases that are difficult if not impossible to meet with SCORM. These cases prompted ADL to begin looking for alternative options. The initial effort resulted in the xAPI specification. It was intended to resolve issues such as:
- SCORM assumes content is managed and launched by an LMS (Advanced Distributed Learning Initiative, 2012, p. RTE-2-7).
  - Not all content can be contained and managed by an LMS, such as video games, virtual worlds, and simulators.
- SCORM embeds its API in a web page (Advanced Distributed Learning Initiative, 2012, p. RTE-3-28). 
  - Not all content is web-based content. In cases where the content is not web-based, finding the SCORM API is not defined, like mobile apps and platform-based games.
- SCORM has a strictly defined data model (Advanced Distributed Learning Initiative, 2012, p. RTE-4-3).
  - Not every learning experience fits neatly into that model - things like tracking altitude in a flight simulator, or how many times a user paused a video does not translate into the SCORM data model.
- SCORM tracking only applies for an individual learner (Advanced Distributed Learning Initiative, 2012, p. RTE-2-5).
  - Group learning and interactions cannot be recorded with SCORM.
  - Data about instructors, tutors, mentors and peers cannot be stored with SCORM.
- SCORM does not provide guidance on how to get data back out of an LMS (Advanced Distributed Learning Initiative, 2012, p. RTE-2-5).
  - The interface described in SCORM only exists during the life of a sharable content object (SCO) communication session. There is no SCORM API that exists outside of that communication session.
  - Reporting and data access requirements are not standardized in SCORM resulting in proprietary, LMS-specific, reporting functionality that can differ between vendors.

## 3.0 How to Use this Profile
This document is intended to be used in addition to the xAPI specification. All communication, data formats and experiences shall follow the requirements in the xAPI specification. The guidance provided in this document is to add information specifically to support SCORM communications in xAPI format.  

### 3.1 Supporting SCORM Features
SCORM is a mature and formal specification that allows for delivering content and tracking learner’s progress. By identifying the key features in SCORM, it is possible to provide guidance on how to implement the same features with the xAPI, such as:

- Launching and Initializing Content
- Supporting the SCORM Temporal Model
- Mapping the SCORM data model to xAPI Statements
- Course Structure & Rolllup 

### 3.2 General Guidance on Statements
In the xAPI, content providers and content issue statements about the learner’s experiences. These statements hold the data about the experience, such as, if the learner was successful, or did the learner complete the content. Additional information and guidance is provided below. All statements issued shall meet the requirements in the xAPI specification and the guidance described in the following sections.  

#### Internationalized Resource Identifier (IRI)
IRIs are used as identifiers in many parts of the xAPI specification. Their syntax consists of a hierarchical part, which is broken into an authority and a path. The authority part shall represent a domain of your organization - ie. adlnet.gov, army.mil. The path part shall identify the object - /courses/cs/CS101. If you are identifying something that cannot be represented by an fully qualified path, use a tag IRI - tag:adlnet.gov,2013:expapi:0.9:extensions - where the domain is a domain you control, the date is the date this tag was created, the next part is the type - scorm, xapi, the version - 2004, 1.2, 1.0.1, and the last part is the identifier. (see [http://www.ietf.org/rfc/rfc4151.txt](http://www.ietf.org/rfc/rfc4151.txt))  
  
__Good IRIs__  
Good IRIs uniquely identify an object  
  <pre>`http://adlnet.gov/activities/courses/CS/CS101`</pre>
  <pre>`http://mydomain.com/content/understanding-fire-safety`</pre>
__Bad IRIs__  
Bad IRIs are not unique and could conflict with others using the same identifier  
    <pre>`act:my-activity01`</pre>
    <pre>`http://example.com/activity/01`</pre>

#### Actor
The actor property refers to the learner that is interacting with the content. The actor should be able to be mapped back to the learner in the LMS. Any of the Actor formats described in the xAPI specification are allowed. However some formats can potentially expose personally identifiable information, it is recommended to use a format that protects the learner’s information.  
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
Verbs in the xAPI spec represent the action that is being recorded for this experience, such as “read” a book and “answered” a question. The xAPI allows for any verb to be used within a statement assuming it follows the rules defined in the xAPI specification. However this flexibility causes issues when trying to report on the data. For consistency and interoperability, those implementing this profile shall use the verbs maintained by ADL at http://adlnet.gov/expapi/verbs/, and use the definition and usage on the ADL Verb pages as guidance.  
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
    <td><a href="http://adlnet.gov/expapi/verbs/initialized/initialized.html">initialized</a></td>
    <td>Initialize()<br>LMSInitialize()</td>
    <td>Initialization of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/terminated/index.html">terminated</a></td>
    <td>Terminate()<br>LMSFinish()</td>
    <td>Termination of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/suspended/index.html">suspended</a></td>
    <td>cmi.exit=suspend<br>cmi.core.exit=suspend</td>
    <td>Suspension of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/resumed/index.html">resumed</a></td>
    <td>cmi.entry=resume<br>cmi.core.entry=resume</td>
    <td>Resumption of the SCO attempt</td>
    <td>SCO</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/passed/passed.html">passed</a></td>
    <td>cmi.success_status=passed<br>cmi.core.lesson_status=passed</td>
    <td>The leaner's performance on this attempt at least met the minimum threshold for the activity</td>
    <td>course<br>SCO<br>objective<br>interaction</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/failed/index.html">failed</a></td>
    <td>cmi.success_status=failed<br>cmi.core.lesson_status=failed</td>
    <td>The performance on this attempt did not meet the minimum threshold for the activiy</td>
    <td>course<br>SCO<br>objective<br>interaction</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/scored/index.html">scored</a></td>
    <td>cmi.scored.scaled<br>cmi.core.score.raw</td>
    <td>Used when the content wants to record a score without implying a level of satisfaction or completion of the content, or wants to provide evidence toward satisfaction or completion</td>
    <td>course<br>SCO<br>objective<br>interaction</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/completed/index.html">completed</a></td>
    <td>cmi.completion_status=completed<br>cmi.core.lesson_status=completed</td>
    <td>The learner has experienced a suffiecient amount of the activity</td>
    <td>course<br>SCO<br>objective<br>interaction</td>
  </tr>
</table>

#### Object
The object property describes the item with which the learner is interacting. The type of object could be an Activity, an Actor or Group, or a Statement (as StatementRef or Sub-Statement). If it is an Actor or a Statement follow the rules defined in the [xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#414-object). However if it is an Activity, follow the rules in the [xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#4141-when-the-objecttype-is-activity) and the rules outlined below.  

##### Activity
Things like courses, SCOs, and objectives are considered activities. Since these experiences are commonly what SCORM content wants to capture, the following sections detail the recommended usage to create interoperable statements.  

###### Activity IRI
Activity IDs are required to be [IRIs](http://en.wikipedia.org/wiki/Internationalized_resource_identifier) and follow the requirements within the xAPI specification. In addition it is recommended that the IRIs are IRLs and that the location contains the metadata (Activity Definition) of the activity. Additionally activity IRIs should be constructed in a predictable way to make interpreting those IRIs possible in a standard way.  

###### Course IRI
The course IRI may be decided by the organization as long as it follows the IRI rules described in the xAPI spec. The activity provider or content must support the launch courseIRI property, described in the launch section of this document, and update the course IRI to that property value if the local and launch-provided values differ. This allows for content to use the most up-to-date identifier in the context of the current launch.  
  
__Guidelines for Activity IRI Contruction__  
- follow the guidance described in the Internationalized Resource Identifier (IRI) section in this document
- course IRIs should be in the format: <scheme>://<authority>/<course path>
  - Course path is any IRI path to the course activity definition, ie courses/CS/101
- SCO IRIs should be in the format: <courseIRI>/<sco path>
  - SCO path is any IRI path to the SCO activity definition, ie lesson/01
- Interaction IRIs should be in the format: <scoIRI>/interactions/<interaction path>
  - Interaction path is any IRI path to the interaction activity definition, ie multi-choice/07
- Objectives IRIs vary depending on the scope of the objective
  - If the objective is local to the SCO: <scoIRI>/objectives/<objective path>
  - If the objective is available to the entire course: <courseIRI>/objectives/<objective path>
  - If the objective is available to any course (global): <scheme>://<authority>/objectives/<objective path>
  - Objective path is any IRI path to the objective activity definition, ie if-else/

###### Activity Definition
The activity definition provides information about the activity. At a minimum all statements will include an activity definition with a name and description. If the activity is representing a SCORM interaction, follow the rules outlined in the xAPI specification for [interaction activities](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#interactionacts).  
  
Activity definitions that differ from one that an LRS already has stored may not be accepted. It is recommended that organizations ensure that all references to the activity definition contain the same information. It is permissible for organizations to host their activity definitions at the location of the activity IRI. In this case the hosted definition is stored at the activity IRI location (IRL), and is retrievable by the LRS in application/json format. Additionally, the statements will not include the definition and instead expect the LRS to acquire the activity definition from its hosted location.

###### Result
The [result property of a statement](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#415-result) represents the outcome of a learner experience. Statements issued using a result shall follow the requirements listed in the xAPI specification and the additional guidance described in this document.

###### Context
The context property adds additional contextual information about the learner experience. Information like who is the instructor of this content, with what registration this experience is associated, or to what activities this experience is related. Statements shall meet all the requirements in the xAPI specification and the additional guidelines described in this document.

###### Timestamp
The timestamp property gives the activity provider or content the ability to set the date and time when the experience occurred. This is different from the stored timestamp property, which indicates when the LRS saved the statement about the experience. All statements shall set the timestamp property. By doing this, it makes it possible to order statements in the same order as they were submitted by the activity provider or content.

###### Authority
The authority property for each statement is set by the LRS to the agent who submitted the statement. This property can be used to filter statements based on the activity provider or content. By using this property as a GET statements filter, it is possible to narrow down the returned statements to the ones submitted by agents whom your organization trusts.

###### Attachments
Attachments give the ability to include supporting documents or information that do not directly map to a section of an xAPI statement. Examples of attachments include PDF certificates, essays, videos, and the JSON web signature. Attachments are not returned by the LRS by default, but can be included by adding the attachments filter to GET statements requests. [See the xAPI specification for more details](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#signature).

## 4.0 Launching and Initializing Content

## 5.0 Supporting the SCORM Temporal Model

## 6.0 Mapping the SCORM Data Model to xAPI Statements

## 7.0 Retrieving and Interpreting xAPI Statements

## Appendix
### References
Advanced Distributed Learning Initiative. (2012). _SCORM 2004 4th Edition Run-Time Environment (RTE) Version 1.1_. (SCORM 2004 4th Edition Specification). Alexandria, VA: Author. 