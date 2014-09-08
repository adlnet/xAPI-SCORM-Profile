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

## 4.0 Launching and Initializing Content

## 5.0 Supporting the SCORM Temporal Model

## 6.0 Mapping the SCORM Data Model to xAPI Statements

## 7.0 Retrieving and Interpreting xAPI Statements

## Appendix
### References
Advanced Distributed Learning Initiative. (2012). _SCORM 2004 4th Edition Run-Time Environment (RTE) Version 1.1_. (SCORM 2004 4th Edition Specification). Alexandria, VA: Author. 
