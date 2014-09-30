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

## Table of Contents
* [Glossary](#glossary)
* 1.0 [Purpose](#10-purpose)
* 2.0 [When to Use this Profile](#20-when-to-use-this-profile)
* 3.0 [How to Use this Profile](#30-how-to-use-this-profile)
  * 3.1 [Supporting SCORM Features](#31-supporting-scorm-features)
  * 3.2 [General Guidance on Statements](#32-general-guidance-on-statements)
* 4.0 [Launching and Initializing Activities](#40-launching-and-initializing-activities)
  * 4.1 [Web-Based Activities](#41-web-based-activities)
  * 4.2 [LMS-Provided Endpoint](#42-lms-provided-endpoint)
  * 4.3 [Out-of-Band Configuration](#43-out-of-band-configuration)
* 5.0 [Supporting the SCORM Temporal Model](#50-supporting-the-scorm-temporal-model)
* 6.0 [Mapping the SCORM Data Model to xAPI Statements](#60-mapping-the-scorm-data-model-to-xapi-statements)
* 7.0 [Retrieving and Interpreting xAPI Statements](#70-retrieving-and-interpreting-xapi-statements)
* [Appendix](#appendix)
  * [Common Scenarios](#common-scenarios) 
  * [SCORM xAPI Data Model Mapping](#complete-scorm-to-xapi-data-model-mapping)

## Glossary
##### Activity:  
Generally synonymous with content, it is the entity with which the learner interacts - ie: video, lesson, slides, etc.   
##### Activity Provider:  
An application that delivers activities to the learner - ie: browser, e-reader, mobile app, etc.  
##### Experience:  
A notable interaction or event between the learner(s) and an activity - ie: took a test, completed the activity, answered a question, etc.  
##### Experience API (xAPI):  
A specification created to provide a way to represent and report experiences between a learner, or group of learners, and some activity.  
##### Internationalized Resource Identifier (IRI):  
A format of an identifier, related to URI, that supports the Universal Character Set (Unicode). See [RFC 3987](http://tools.ietf.org/html/rfc3987).  
##### Statement:  
The xAPI data format for representing learner experience data. See [xAPI Statement](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#statement).

## 1.0 Purpose
The Experience API (xAPI) was created in response to the eLearning community’s desire to modernize the Sharable Content Object Reference Model (SCORM) capabilities, which was initially developed to make courseware interoperable with learning management systems. Since its introduction in 2000, SCORM has played a critical role in the proliferation of online training and education courses. The Experience API introduces a new paradigm for tracking and recording learning-related data. Learners can be tracked as they perform work tasks, produce work outputs, communicate, collaborate, and engage in just about any other online activity. This API uses a flexible data format that supports many different use cases and needs. However with this flexibility arises the need for developers to formalize the data they track and what that data means. By formalizing the data reported by content and the format of that data, tools can be created that can make sense of that data.

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
This document is intended to be used in addition to the [xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md). All communication, data formats and experiences shall follow the requirements in the xAPI specification. The guidance provided in this document is to add information specifically to support SCORM communications in xAPI format.  

### 3.1 Supporting SCORM Features
SCORM is a mature and formal specification that allows for delivering content and tracking learner’s progress. By identifying the key features in SCORM, it is possible to provide guidance on how to implement the same features with the xAPI, such as:

- Launching and Initializing Content
- Supporting the SCORM Temporal Model
- Mapping the SCORM data model to xAPI Statements

### 3.2 General Guidance on Statements
In the xAPI, activity providers and activities issue statements about the learner’s experiences. These statements hold the data about the experience, such as, if the learner was successful, or did the learner complete the content. Additional information and guidance is provided below. All statements issued shall meet the requirements in the xAPI specification and the guidance described in the following sections.  

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
The actor property refers to the learner that is interacting with the activity. The actor should be able to be mapped back to the learner in the LMS. Any of the Actor formats described in the xAPI specification are allowed. However some formats can potentially expose personally identifiable information, it is recommended to use a format that protects the learner’s information.  
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
Verbs in the xAPI spec represent the action that is being recorded for this experience, such as "read" a book and "answered" a question. The xAPI allows for any verb to be used within a statement assuming it follows the rules defined in the xAPI specification. However this flexibility causes issues when trying to report on the data. For consistency and interoperability, those implementing this profile shall use the verbs maintained by ADL at http://adlnet.gov/expapi/verbs/, and use the definition and usage on the ADL Verb pages as guidance.  
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
    <td>The performance on this attempt did not meet the minimum threshold for the activity</td>
    <td>course<br>SCO<br>objective<br>interaction</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/scored/index.html">scored</a></td>
    <td>cmi.scored.scaled<br>cmi.core.score.raw</td>
    <td>Used when the activity wants to record a score without implying a level of satisfaction or completion of the activity, or wants to provide evidence toward satisfaction or completion</td>
    <td>course<br>SCO<br>objective<br>interaction</td>
  </tr>
  <tr>
    <td><a href="http://adlnet.gov/expapi/verbs/completed/index.html">completed</a></td>
    <td>cmi.completion_status=completed<br>cmi.core.lesson_status=completed</td>
    <td>The learner has experienced a sufficient amount of the activity</td>
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
  - Objective path is any IRI path to the objective activity definition, ie `if-else/`

###### Activity Definition
The activity definition provides information about the activity. At a minimum all activities should include an activity definition with a name and description. If the activity is representing a SCORM interaction, follow the rules outlined in the xAPI specification for [interaction activities](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#interactionacts).  
  
Activity definitions that differ from one that an LRS already has stored may not be accepted. It is recommended that organizations ensure that all references to the activity definition contain the same information. It is permissible for organizations to host their activity definitions at the location of the activity IRI. In this case the hosted definition is stored at the activity IRI location (IRL), and is retrievable by the LRS in application/json format. Additionally, the statements will not include the definition and instead expect the LRS to acquire the activity definition from its hosted location.

###### Result
The [result property of a statement](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#415-result) represents the outcome of a learner experience. Statements issued using a result shall follow the requirements listed in the xAPI specification and the additional guidance described in this document.

###### Context
The context property adds additional contextual information about the learner experience. Information like who is the instructor of this content, with what registration this experience is associated, or to what activities this experience is related. Statements shall meet all the requirements in the xAPI specification and the additional guidelines described in this document.

###### Timestamp
The timestamp property gives the activity provider or activity the ability to set the date and time when the experience occurred. This is different from the stored timestamp property, which indicates when the LRS saved the statement about the experience. All statements shall set the timestamp property. By doing this, it makes it possible to order statements in the same order as they were submitted by the activity provider or activity.

###### Authority
The authority property for each statement is set by the LRS to the agent who submitted the statement. This property can be used to filter statements based on the activity provider or activity. By using this property as a GET statements filter, it is possible to narrow down the returned statements to the ones submitted by agents whom your organization trusts.

###### Attachments
Attachments give the ability to include supporting documents or information that do not directly map to a section of an xAPI statement. Examples of attachments include PDF certificates, essays, videos, and the JSON web signature. Attachments are not returned by the LRS by default, but can be included by adding the attachments filter to GET statements requests. [See the xAPI specification for more details](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#signature).

## 4.0 Launching and Initializing Activities
In a SCORM environment, an activity is launched by the LMS. The LMS can provide activities with launch parameters and initial data model values. This helps a SCO to work correctly in the current environment. In comparison, xAPI activities can exist outside of an LMS - information such as learner name and identifier, or the location of the xAPI LRS may be unknown at the launch of the activity.

Solutions such as Rustici Software Launch, AICC CMI5 and IMS LTI all provide ways for systems, such as an LMS, to launch and initialize activities that are not managed by the LMS. Any of those solutions may be leveraged to solve specific, individual issues that are not addressed in this document. At a minimum the following requirements will be used for launching and initializing activities:  
- entry: ab-initio or resume
- endpoint: LRS endpoint 
- actor: Agent Account
  - Account Homepage: location of the LMS that holds the user account information
  - Account Name: an identifier for the learner that is at least unique to the scope of the LMS
    - It is recommended that the name is does not contain personally identifiable information such as the learner’s name
- courseiri: IRI that identifies the entire course within at least the context of the LMS
  - It is recommended that this identifier is unique and defined by a domain which your organization controls. ex: `http://adlnet.gov/courses/compsci/xxx`  

Since xAPI activites do not need to be web-based, the means of initializing the activities will vary. Following are three strategies:  

### 4.1 Web-Based Activities
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

### 4.2 LMS-Provided Endpoint
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

### 4.3 Out-of-Band Configuration
If the above launch options are not possible developers can preconfigure the activity provider or allow for user input to configure the launch parameters. Work with the LMS and LRS providers for configuration details.  

## 5.0 Supporting the SCORM Temporal Model
SCORM has a temporal model which describes interaction states such as an attempt and a session. The xAPI uses an Activity Stream style model where experiences are all reported to the stream without a sense of session or attempt. This does not mean, however, that xAPI statements cannot be related to one another. By properly using the context attribute of a Statement it is possible to group Statements using the registration ID or broader activity IDs.

To initialize an attempt on a SCO or course component, send a statement with the initialized ADL Verb, the object ID set to the IRI for this SCO, and the context activities parent activity of the course activity IRI to the LRS. When a new session is initialized, generate an attempt GUID, append it to the SCO IRI as an attemptId query parameter and include this attempt-specific SCO IRI to the context activities grouping array. ([See an example in the Appendix](#initialize-a-sco-attempt))

During the session, Statements are collected and sent to the LRS much like SCORM SCOs reporting to the LMS. Statements can be sent to the LRS either immediately or collected and sent as a bundle.

To indicate termination of the SCO attempt, send a statement with the terminated ADL Verb, the object ID set to the IRI for this SCO, and the context activity's parent activity set to the course activity IRI. 

To suspend a SCO session, send a statement with the suspended ADL Verb, the object ID set to the IRI for this SCO, and the context activity's parent activity set to the course activity IRI. And to resume the SCO session, send a statement with the resumed ADL Verb, the object ID set to the IRI for this SCO, and the context activity's parent activity set to the course activity IRI. Continue to use the same attemptId query parameter as generated during the initialization process.

To find the learner’s experiences of the latest attempt, query the LRS for statements with an activity ID of the SCO IRI. Since the LRS returns statements ordered by descending stored time, the first statement in the list should be from the latest attempt. Use the context activity's grouping SCO IRI, with the attemptId query parameter, to query the LRS again for all statements with a related activity ID of that attempt SCO IRI.  

## 6.0 Mapping the SCORM Data Model to xAPI Statements
The following is a list of SCORM data model elements and the equivalent xAPI statement. Using this mapping will allow systems to interpret the xAPI statements in an interoperable way. A complete list of SCORM data model elements and their mapping to xAPI is listed in the [Appendix](#complete-scorm-to-xapi-data-model-mapping).  

#### Entry
Entry is used to indicate the attempt state of the activity - is this a new attempt on the activity or a continuation of the previous attempt? There is no direct mapping to an xAPI statement such as “actor entered activity with result ab-initio”. Instead this is implied by issuing a statement with the ADL Verb `initialized` and a new attemptId on the grouping activity. 

__SCORM 2004:__ `cmi.entry=ab-initio`  
__SCORM 1.2:__ `cmi.core.entry=ab-initio`  
__Experience API Statement:__
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/initialized",
        "display": {
            "en-US": "initialized"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    }
}
```  

#### Exit
Exit is used to indicate the attempt state at the end of the session. It is possible to send an xAPI exited statement but in most cases the intended meaning is that the session has terminated in either a suspended or terminated state. This is accomplished by issuing a statement with the ADL Verb `suspended` and maintaining the current attemptId for future sessions, or with the ADL Verb `terminated` and a new attemptId for future sessions.

__SCORM 2004:__ `cmi.exit=normal`  
__SCORM 1.2:__ `cmi.core.exit=""`  
__Experience API Statement:__
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/terminated",
        "display": {
            "en-US": "terminated"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    }
}
```  

#### Success
Success indicates whether the learner’s attempt was successful. Use of success is recommended. To report the success, the statement verb will be set to the ADL Verb `passed` or `failed` depending on the learner’s success. When applied to courses, SCOs and objectives, the success value is the determining factor of whether or not the learner passed the current activity attempt. This value directly maps to the SCORM data model element lesson_status or success_status.

__SCORM 2004:__ `cmi.success_status=passed`  
__SCORM 1.2:__ `cmi.core.lesson_status=passed`  
__Experience API Statement:__
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/passed",
        "display": {
            "en-US": "passed"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    }
}
```  

#### Completion
Completion indicates if the learner has completed the activity. Setting completion is optional. If it is not set, completion will be assumed by the success status of the activity. If the activity has a success status, completion is assumed to be complete; if the activity does not have a success status, completion is assumed to be incomplete. If it is determined that the activity must have a completion status, set it explicitly in one of two ways - either as a separate completed statement, or as the result of a success status statement.

__SCORM 2004:__ `cmi.completion_status=completed`  
__SCORM 1.2:__ `cmi.core.lesson_status=completed`  
__Experience API Statement:__  
_Statement with Completed as the Verb_  
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/completed",
        "display": {
            "en-US": "completed"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    }
}
```  
_Statement with Completion in the Result_
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/passed",
        "display": {
            "en-US": "passed"
        }
    },
    "result": {
        "completion": true
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    }
}
```  

#### Score
Scores may be reported as supporting information about the learner’s attempt. Since reporting systems may not have a passing threshold to compare the scores the activity provider or activity cannot imply success or completion of an activity solely based on a score. If the score is used for determination of success, this should be evaluated by the activity provider or activity and the relevant success or completion statement should be issued.  
  
> NOTE: For compatibility between SCORM versions, the value of SCORM 1.2 cmi.core.score.raw divided by 100, and of SCORM 2004 cmi.score.scaled should be reported to xAPI as score.scaled. All other scores -  raw, min, max - may be reported directly as xAPI score.raw, score.min, and score.max.  

__SCORM 2004:__ `cmi.score.scaled=0.95`  
__SCORM 1.2:__ `cmi.core.score.raw=95`  
__Experience API Statement:__
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/scored",
        "display": {
            "en-US": "scored"
        }
    },
     "result" : {
        "score" : {
            "scaled" : 0.95
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    }
}
```  

#### Note About Default Status
It was a common practice to set a default status on SCOs in SCORM. Sometimes variations in how an LMS handled a status, if not reported by the activity, caused issues in getting consistent results. Developers often tried to resolve this default status by setting the status in the activity upon launch. Many SCOs at launch will set completion status to incomplete, success status to failed and a score scaled to 0.0. This often works because the LMS is only required to save the latest value for an element of the latest attempt. But since an LRS keeps all records it can start to cause conflicts or confusing results. So to try to prevent these issues, it is strongly recommended not to report a default status.  

#### Interactions
Interactions can be recorded using the xAPI. Interactions can be described in the xAPI using a predefined format that maps to SCORM interactions. Activity providers and activities shall use the ADL Verb `http://adlnet.gov/expapi/verbs/responded`, the result.response attribute of a statement for the response, and an activity definition as described in the xAPI specification to report the learner’s responses for an interaction. See the [interaction activities](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#interaction-activities) section and the [interaction appendix](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#AppendixC) for more details.

> NOTE: The IRI used to identify an interaction is recommended to follow the format: `<courseiri>/<scoid>/interaction/<interactionid>`  

#### Objectives
Objectives are represented as another activity. As such, statements about a learner’s score, success or completion status are reported just as they are for any other activity. But determining a consistent naming scheme for the objective ID makes it easier for reporting systems to understand the statements. For consistency the following guidance is recommended:
- The combination of course IRI, SCO ID, and objective ID should consistently and uniquely identify a single objective
- Use the following IRI syntax when defining an objective:
 - Local objective (SCO or activity level): `<courseIRI>/<SCOID>/objective/<objectiveID>`
 - Course objective (course level): `<courseIRI>/objective/<objectiveID>`
 - Global objective (for all courses): `<user defined IRI>/objective/<objective ID>`

> NOTE: To indicate where this objective statement occurred, include the SCO IRI in the context activities parent array.  

__SCORM 2004:__ `cmi.objectives.n.success_status=passed`  
__SCORM 1.2:__ `cmi.objectives.n.status=passed`  
__Experience API Statement:__  
_Statement with Local Objective_  
``` javascript
{
   "actor":{
      "account":{
         "homePage":"http://lms.adlnet.gov/",
         "name":"500-627-490"
      }
   },
   "verb":{
      "id":"http://adlnet.gov/expapi/verbs/passed",
      "display":{
         "en-US":"passed"
      }
   },
   "object":{
      "id":"http://adlnet.gov/courses/compsci/CS204/lesson01/01/objective/obj-if-else",
      "definition":{
         "name":{
            "en-US":"If-Else Blocks"
         },
         "description":{
            "en-US":"Show proficiency in creating if-else blocks"
         }
      }
   },
   "context":{
      "contextActivities":{
         "parent":[
            {
               "id":"http://adlnet.gov/courses/compsci/CS204/"
            },
            {
               "id":"http://adlnet.gov/courses/compsci/CS204/lesson01/01"
            }
         ],
         "grouping":[
            {
               "id" : "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
            }
         ]
      }
   }
}
```  
_Statement with Sequencing and Navigation Global Objective_  
``` javascript
{
   "actor":{
      "account":{
         "homePage":"http://lms.adlnet.gov/",
         "name":"500-627-490"
      }
   },
   "verb":{
      "id":"http://adlnet.gov/expapi/verbs/passed",
      "display":{
         "en-US":"passed"
      }
   },
   "object":{
      "id":"http://myurl.example.com/objective/global-obj-if-else",
      "definition":{
         "name":{
            "en-US":"If-Else Blocks"
         },
         "description":{
            "en-US":"Show proficiency in creating if-else blocks"
         }
      }
   },
   "context":{
      "contextActivities":{
         "parent":[
            {
               "id":"http://adlnet.gov/courses/compsci/CS204/"
            },
            {
               "id":"http://adlnet.gov/courses/compsci/CS204/lesson01/01"
            }
         ],
         "grouping":[
            {
               "id" : "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
            }
         ]
      }
   }
}
```
## 7.0 Retrieving and Interpreting xAPI Statements
Storing learning experiences in an xAPI LRS is not the only consideration. Retrieving the learning experiences and interpreting what those statements mean is another aspect that needs guidance for consistent reporting and tracking. The following sections discuss the processes to identify results from activities and their meaning.

### Trusting the Statements
Determining which statements are trusted and authoritative is open to the individual organization. The following are a few strategies that can be applied to your organizations’ needs.

#### Private LRS
The safest way to ensure that the available statements in an LRS are trustworthy is to host an LRS within your organization. By controlling the environment that hosts the LRS, you ensure that the data contained in the LRS was submitted by authorized activity providers and activities. In this case, no other evaluation of the statements is necessary.

#### Authority
If the LRS is publicly hosted, the first way to identify trusted statements is to search based on the authority value. Each statement stored in an LRS has an authority value set to the agent who submitted the statement. This value is set by the LRS, and is based on the agent associated with the credentials used to submit the statement. A system wishing to only retrieve statements issued by certain activity providers or activities can filter the LRS results using those providers’ agent information.  
  
##### Retrieving Statements Based on the Authority
__Decoded:__  
<pre>
GET  
statements?agent={"account":{"homePage":"http://adlnet.gov/accounts/","name":"449-002"}}&related_agents=true
</pre>  
  
__Encoded:__  
<pre>
GET  
statements?agent=%7B%E2%80%9Caccount%E2%80%9D%3A%7B%E2%80%9ChomePage%E2%80%9D%3A%E2%80%9Chttp%3A%2F%2Fadlnet.gov%2Faccounts%2F%E2%80%9D%2C%E2%80%9Cname%E2%80%9D%3A%E2%80%9C449-002%E2%80%9D%7D%7D&related_agents=true
</pre>
#### Signed Statements
Another option for identifying the authenticity of a statement is by issuing signed statements. A statement is signed by an activity provider attaching a JSON web signature to the statement. This digital signature can be retrieved from the LRS with the statement and verified before considering the statement in the results. See the xAPI Specification for [how to sign](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#44-signed-statements) and [retrieve signed statements](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#723-getstatements).

##### Retrieving Statements with their Attachments
<pre>
GET  
statements?attachments=true
</pre>

### Determining Status
Due to activity providers and activities reporting to an LRS instead of an LMS, the determination of status might not be decided by an LMS. An LMS may still be used for the determination of a learner’s status, but it may also be determined by the activity provider or activities, reporting tools, or other client applications consuming the LRS data. 

The method of determining status is up to those developing the activities. For example, activities within a course may report individual status statements and leverage a final piece of content to look at the collection of results and report an overall status for the course. Another example is a reporting tool customized with an organization’s accepted passing threshold collecting scores from a course and evaluating them based on the organization threshold. The results of that evaluation could be reported to the LRS as the success for the course.  

### Resolving Conflicts
Statements in an LRS are stored as a stream of information. It is possible to retrieve statements that conflict. It might be that a learner initially failed a test but later tried again and passed. In the LRS that result would likely appear as two separate, and conflicting, statements. Another issue that might arise is that the SCOs or lessons within a course may report one result while the course reports an overall status of another result. The following rules shall be followed to resolve such conflicts.

#### Granularity
It is possible to report status of a course in three different levels of granularity: course, SCO/lesson, objective. Since an LRS makes no assumptions about the statements it receives and has no rules about how to evaluate the statements contained within, it is recommended that all evaluation, rollup of results and final course status be reported by the activity provider, the activity or a trusted evaluation tool.  
  
For determining status based on granularity:
- If a system consuming the statements finds a course status, it shall use that result. 
- If no course status is found consuming tools may use the status of all activities that include the course IRI as a parent activity in the context activities property.
- And at the most granular level, the consuming tools may use the status of all of the objectives for each of the activities that include the course IRI as a parent activity in the context activities property.  
  
#### Status
It is recommended that all activities report as much information about a learner’s status in an activity as they can.  
- At a minimum, activities should report a success status, such as passed or failed. 
  - If a success status does not make sense for the type of activity, the activity should report completion status of completed when it is determined the learner has completed the activity. 
  - The activity may report a score.
- For tools using these results from the LRS, the activity status is first based on the success status, if found. 
  - If there is no success status, the tool may then use the completion status. 
  - If there is no success or completion status the tool may use the score. 
    - If the tool can determine success from the score, i.e., it has a threshold to compare with the score, it is permitted to use that evaluation.

#### Authority
The authority property of a statement identifies the agent who submitted a particular statement. This agent could be the activity provider, the activity, a teacher or a reporting tool. It is up to the organization to choose which agents are to be considered when determining status. Querying the LRS can be narrowed down by authority by using the approved agent id and requesting related agents.  
__Decoded__
<pre>
GET  
statements/?agent={"id":"myactivityprovider@mycompany.com"}&related_agents=true
</pre>

#### Attempt
If the activity was following the SCORM temporal model, it may be necessary to resolve conflicts by only using results from the latest attempt. You can find the latest attempt by querying the LRS for all statements for a learner in a course:  
__Decoded__
<pre>
GET  
statements/?agent={"account":{"homePage":"http://mycontent.example.com", "name":"334-009"}&activity=http://example.com/cs400/lesson01
</pre>
From those results, find the latest statements by ordering them by the timestamp property and then using the statements with the attemptId from the latest statement.

#### Time
All statements submitted by activity providers and activities following this profile are required to include a timestamp. Use of this timestamp property allows systems to create a timeline of the statements. Using this timeline can help resolve conflicts where the same property for the same granularity, authority, and attempt exist. In this case the most recent statement takes precedence. For example, if a SCO starts and reports that the learner failed this attempt, then the learner takes a test and passes this attempt, since the passed statement is the most recent, tools will use this one for the status of the attempt.  

## Appendix

### Common Scenarios

#### Initialize a SCO Attempt
__SCORM 2004:__ Initialize()  
__SCORM 1.2:__ LMSInitialize()  
__xAPI:__ actor 500-627-490 initialized lesson01 with attempt id (x) in course CS204  
``` javascript
{
   "actor":{
      "account":{
         "homePage":"http://lms.adlnet.gov/",
         "name":"500-627-490"
      }
   },
   "verb":{
      "id":"http://adlnet.gov/expapi/verbs/initialized",
      "display":{
         "en-US":"initialized"
      }
   },
   "object":{
      "id":"http://adlnet.gov/courses/compsci/CS204/lesson01/01",
      "definition":{
         "name":{
            "en-US":"lesson 01"
         },
         "description":{
            "en-US":"The first lesson of CS204"
         }
      }
   },
   "context":{
      "contextActivities":{
         "parent":[
            {
               "id":"http://adlnet.gov/courses/compsci/CS204/"
            }
         ],
         "grouping":[
            {
               "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
            }
         ]
      }
   },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```  
#### Terminate a SCO
__SCORM 2004:__ Terminate()  
__SCORM 1.2:__ LMSFinish()  
__xAPI:__ actor 500-627-490 terminated lesson01 with attempt id (x) in the course CS204
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/terminated",
        "display": {
            "en-US": "terminated"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Suspend a SCO
__SCORM 2004:__ cmi.exit=suspend  
__SCORM 1.2:__ cmi.core.exit=suspend  
__xAPI:__ agent 500-627-490 suspended lesson01 with attempt id (x) in the course CS204  
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/suspended",
        "display": {
            "en-US": "suspended"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Resume a SCO
__SCORM 2004:__ cmi.entry=resume  
__SCORM 1.2:__ cmi.core.entry=resume  
__xAPI:__ agent 500-627-490 resumed lesson01 with same attempt id in the course CS204
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/resumed",
        "display": {
            "en-US": "resumed"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Set a SCO to passed
__SCORM 2004:__ cmi.success_status=passed  
__SCORM 1.2:__ cmi.core.lesson_status=passed  
__xAPI:__ agent 500-627-490 passed lesson01 with attempt id (x) in the course CS204
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/passed",
        "display": {
            "en-US": "passed"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Set the score of a SCO
__SCORM 2004:__ cmi.score.scaled=0.85  
__SCORM 1.2:__ cmi.core.score.raw=85 [NOTE: raw is normalized to 0-100. Convert it to a decimal by dividing raw by 100]  
__xAPI:__ agent 500-627-490 scored 0.85 on attempt id (x) in the course CS204  
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/scored",
        "display": {
            "en-US": "scored"
        }
    },
    "result" : {
        "score" : {
            "scaled" : 0.85
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Set a SCO to completed
__SCORM 2004:__ cmi.completion_status=completed  
__SCORM 1.2:__ cmi.core.lesson_status=completed  
__xAPI:__ agent 500-627-490 completed lesson01 with attempt id (x) in the course CS204
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/completed",
        "display": {
            "en-US": "completed"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
               "en-US" : "lesson 01"
            },
            "description" : {
               "en-US" : "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Shorthand for completion, success and score for a SCO  
__SCORM 2004:__  
- cmi.completion_status=completed  
- cmi.success_status=passed  
- cmi.score.scaled=0.85  

__SCORM 1.2:__   
- cmi.core.lesson_status=passed  
- cmi.core.score.raw=85  

> NOTE: A lesson status of passed implies completed, however completed lesson status does not imply passed. The cmi.core.lesson_status value must be passed for it to reflect this shorthand.  

__xAPI:__ agent 500-627-490 passed lesson01 of attempt id (x) in the course CS204 with a score of 0.85 and completion true  
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/passed",
        "display": {
            "en-US": "passed"
        }
    },
    "result": {
        "score": {
            "scaled": 0.85
        },
        "completion": true
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01",
        "definition": {
            "name": {
                "en-US": "lesson 01"
            },
            "description": {
                "en-US": "The first lesson of CS204"
            }
        }
    },
    "context": {
        "contextActivities": {
            "parent": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/"
                }
            ],
            "grouping": [
                {
                    "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
                }
            ]
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Set the course to passed
__xAPI:__ agent 500-627-490 passed the course CS204
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/passed",
        "display": {
            "en-US": "passed"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/",
        "definition": {
            "name": {
                "en-US": "CS204"
            },
            "description": {
                "en-US": "The CS204 course"
            }
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Set a score for the course
__xAPI:__ agent 500-627-490 scored 0.85 in the course CS204  
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/scored",
        "display": {
            "en-US": "scored"
        }
    },
    "result" : {
        "score" : {
            "scaled" : 0.85
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/",
        "definition": {
            "name": {
                "en-US": "CS204"
            },
            "description": {
                "en-US": "The CS204 course"
            }
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Set course as completed
__xAPI:__ agent 500-627-490 completed the course CS204  
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/completed",
        "display": {
            "en-US": "completed"
        }
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/",
        "definition": {
            "name": {
                "en-US": "CS204"
            },
            "description": {
                "en-US": "The CS204 course"
            }
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Shorthand for passed the course with a score and completion value
__xAPI:__ agent 500-627-490 passed the course CS204 with a score of 0.85 and completion true
``` javascript
{
    "actor": {
        "account": {
            "homePage": "http://lms.adlnet.gov/",
            "name": "500-627-490"
        }
    },
    "verb": {
        "id": "http://adlnet.gov/expapi/verbs/passed",
        "display": {
            "en-US": "passed"
        }
    },
    "result": {
        "score": {
            "scaled": 0.85
        },
        "completion": true
    },
    "object": {
        "id": "http://adlnet.gov/courses/compsci/CS204/",
        "definition": {
            "name": {
                "en-US": "CS204"
            },
            "description": {
                "en-US": "The CS204 course"
            }
        }
    },
   "timestamp":"2014-08-01T15:05:04-04:00"
}
```

#### Group Statements by Attempt ID
- On Initialize of an attempt generate an attempt guid
  - `50fd6961-ab6c-4e75-e6c7-ca42dce50dd6`
- Append the attemptId url query parameter to SCO IRI
  - `http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6`
- Include the attempt SCO IRI in the context activities grouping list  
``` javascript
 ...,
 "context": {
      "contextActivities": {
          "parent": [
              {
                  "id": "http://adlnet.gov/courses/compsci/CS204/"
              }
          ],
          "grouping": [
              {
                  "id": "http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
              }
          ]
      }
  },
  ...
```  
- On Terminate of an attempt clear the attempt guid

#### Find Statements by Attempt ID
- append the attemptId url query parameter to the SCO IRI  
  - `http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6`
- issue a get statements request to the LRS with activity request parameter set to the attempt SCO IRI, and related_activities request parameter set to true   
 
_Unencoded for readability_  
```
GET  
statements/?activity=http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6&related_activities=true
```  

#### Find the Latest Attempt ID  
- Issue a get statements request to the LRS with the activity request parameter set to the SCO IRI  
 
_Unencoded for readability_  
```
GET  
statements/?activity=http://adlnet.gov/courses/compsci/CS204/lesson01/01
```  

- find the context activities grouping id set to the attempt SCO IRI of the first statement in the StatementResults array returned from the LRS  

#### Find all Statements from the Latest Attempt  
- Issue a get statements request to the LRS with the activity request parameter set to the SCO IRI
 
_Unencoded for readability_  
```
GET  
statements/?activity=http://adlnet.gov/courses/compsci/CS204/lesson01/01
```  

- Find the context activities grouping id set to the attempt SCO IRI of the first statement in the StatementResults array returned from the LRS
- Issue a get statements request to the LRS with the activity request parameter set to the attempt SCO IRI, and related_activities request parameter set to true  
 
_Unencoded for readability_  
```
GET  
statements/?activity=http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=[attempt guid]&related_activities=true
```  


### Complete SCORM to xAPI Data Model Mapping
#### Comments From Learner
SCORM 1.2 Comments and SCORM 2004 Comments from Learner mapped to an Experience API Statement. The `commented` ADL Verb is used with the comment as the xAPI Statement result response value. For SCORM 2004 where there is also a timestamp and a location, use the statement timestamp attribute for the comment timestamp value and the Activity URI as the location.  
__SCORM 2004:__ `cmi.comments_from_learner`  
__SCORM 1.2:__ `cmi.comments`  
__Experience API Statement:__   
``` javascript
{
   "actor":{
      "account":{
         "homePage":"http://lms.adlnet.gov/",
         "name":"500-627-490"
      }
   },
   "verb":{
      "id":"http://adlnet.gov/expapi/verbs/commented",
      "display":{
         "en-US":"commented"
      }
   },
   "object":{
      "id":"http://adlnet.gov/courses/compsci/CS204/lesson01/01",
      "definition":{
         "name":{
            "en-US":"lesson 01"
         },
         "description":{
            "en-US":"The first lesson of CS204"
         }
      }
   },
   "result":{
      "response":"This is a great lesson. You do such a wonderful job teaching!"
   },
   "timestamp":"2014-09-29T18:18:24.316Z",
   "context":{
      "contextActivities":{
         "parent":[
            {
               "id":"http://adlnet.gov/courses/compsci/CS204/"
            }
         ],
         "grouping":[
            {
               "id":"http://adlnet.gov/courses/compsci/CS204/lesson01/01?attemptId=50fd6961-ab6c-4e75-e6c7-ca42dce50dd6"
            }
         ]
      }
   }
}
```  

#### Comments From LMS
Comments From LMS allows an activity to see comments about the content. The value is the same for all learners, and is made available for each activity. For those reasons, Comments From LMS is available at the Activity Profile endpoint.  
__SCORM 2004:__ `cmi.comments_from_lms`  
__SCORM 1.2:__ `cmi.comments_from_lms`  
__Experience API:__  
[Activity Profile Endpoint](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#actprofapi)  
Activity ID: The activity IRI  
Profile ID: http://adlnet.gov/xapi/profile/scorm/activity-profile  
See [SCORM Activity Profile Object](#scorm-activity-profile) for object format.  

#### Completion Status
See [Completion](#completion)  

#### Completion Threshold

#### Credit

#### Entry

#### Exit

#### Interactions

#### Launch Data

#### Learner ID

#### Learner Name

#### Learner Preferences
##### Audio Level
##### Language
##### Delivery Speed
##### Audio Captioning

#### Location

#### Max Time Allowed

#### Mode

#### Objectives

#### Progress Measure

#### Scaled Passing Score

#### Score

#### Session Time

#### Success Status

#### Suspend Data

#### Time Limit Action

#### Total Time

#### ADL Data

### XAPI SCORM Data Objects
#### SCORM Activity Profile
<table>
<tr><th>Property</th><th>Description</th></tr>
<tr>
 <td>comments_from_lms</td>
 <td><a href="#scorm-activity-profile-comment-object">SCORM Activity Profile Comment Object</a></td>
</tr>
</table>

#### SCORM Activity Profile Comment Object
<table>
<tr><th>Property</th><th>Description</th></tr>
<tr>
 <td>comment</td>
 <td>String</td>
</tr>
<tr>
 <td>location</td>
 <td>String</td>
</tr>
<tr>
 <td>timestamp</td>
 <td>Timestamp <a href="https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md#417-timestamp">ISO 8601</a></td>
</table>

### References
Advanced Distributed Learning Initiative. (2012). _SCORM 2004 4th Edition Run-Time Environment (RTE) Version 1.1_. (SCORM 2004 4th Edition Specification). Alexandria, VA: Author. 
