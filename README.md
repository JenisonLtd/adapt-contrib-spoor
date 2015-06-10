adapt-contrib-spoor
===================
Tracking plugin for the Adapt Framework. Currently (officially) only supports SCORM 1.2

##Installation
If you haven't already done so, be sure to install the [Adapt Command Line Interface](https://github.com/adaptlearning/adapt-cli) then, from the command line run:-
```
$ adapt install adapt-contrib-spoor
```

This component can also be installed by adding the component to the adapt.json file before running `adapt install`:
```
"adapt-contrib-spoor": "*"
```

##Usage instructions
You will need to add the data that is in [example.json](example.json) to your config.json, amending the settings as required (see below).

Additionally, you must update your course to include tracking IDs in blocks.json - this should be done by running
```
$ grunt tracking-insert
```
from the command line. N.B. if you add more blocks, you will need to run this again to assign tracking ids to the new blocks.

###SCORM manifest file
You will also need to edit the manifest file [imsmanifest.xml](required/imsmanifest.xml) to contain information specific to your course.

A full description of the many ways this file can be set up and populated is beyond the scope of this README (see the [SCORM 1.2 documentation](http://www.adlnet.gov/resources/scorm-1-2-specification/), specifically the SCORM Content Aggregation Model (SCORM_1.2_CAM) for a full description) - however, at the very least you will likely want to change the course title (currently set to 'Adapt SCORM') and description (currently set to 'Responsive SCORM generated by the Adapt Framework').

##Settings overview
 
A complete example of this plugin's settings can be found in the [example.json](example.json) file.

##Settings explained
####_isEnabled
If set to `true`, the plugin will try to connect to a SCORM conformant LMS on course launch and perform tracking.

If you wish to switch off tracking without uninstalling the plugin - for example when testing locally with no LMS present - set this to `false`.

####_tracking
This section lists completion criteria and other tracking features such as whether to save a score back to the LMS or not.

#####_requireCourseCompleted
If set to `true`, the plugin will require that the user must complete all the components in the course before the course can be marked as finished in the LMS.

Note that if this setting and `_requireAssessmentPassed` (see below) are both set to `true`, the user must pass the course assessment as well as complete all components in order to meet the completion criteria.

#####_requireAssessmentPassed
If set to `true`, the plugin will require that the user must pass the course assessment before the course can be marked as finished in the LMS.

Note that if this setting and `_requireCourseCompleted` are both set to `true`, the user must complete all the components in the course as well as pass the assessment in order to meet the completion criteria.

#####_shouldSubmitScore
If set to `true`, the score attained in any assessment will be reported back to the LMS (regardless of whether the user passes or fails the assessment).

####_reporting
Defines what status to report back to the LMS

#####_onTrackingCriteriaMet
What status to report back to the LMS when the tracking criteria are met.

Valid values are: 'completed', 'passed', 'failed', and 'incomplete' (note - these must be lowercase).

Under most circumstances, if you are tracking a course by assessment, you would set this to 'passed'. Otherwise 'completed' is the usual value to use.

#####_onAssessmentFailure
What status to report back to the LMS when the assessment is failed.

Some Learning Management Systems will prevent the user from making further attempts at the course after it has been set to 'failed', it's therefore common to set this to 'incomplete' to allow the user more attempts to pass the assessment.

###Advanced Settings
You only need to include this section if you want to change any of the following settings from the default. You only need to include settings you want to change.

####_scormVersion
Defines what version of SCORM you're targeting. Note that currently only SCORM 1.2 is officially supported. SCORM 2004 should work OK but we don't test this functionality. If you want to enable SCORM 2004 support, you will not only need to change this to "2004" - but also include the relevant SCORM 2004 packaging files (imsmanifest.xml and others).
Default: "1.2"

####_showDebugWindow
If set to true, a popup window will be shown on course launch that gives detailed information about what SCORM calls are being made. This can be very useful for debugging SCORM issues. Note that this popup window will appear automatically if the SCORM code encounters an error, even if this is set to ```false```.
Default: `false`

####_commitOnStatusChange
Whether a 'commit' call should be made automatically every time the lesson_status is changed or not.
Default: `true`

####_timedCommitFrequency
The frequency (in minutes) at which a 'commit' call should be made automatically. Set to 0 to disable automatic commits altogether.
Default: `10`

####_maxCommitRetries
If a 'commit' call fails, this setting controls how many times it should be retried before giving up and throwing an error.
Default: `5`

####_commitRetryDelay
How much of a delay (in milliseconds) to leave between commit retries.
Default: `2000`

#####References
This plugin uses the excellent :sparkles: [pipwerks SCORM API Wrapper](https://github.com/pipwerks/scorm-api-wrapper/) for JavaScript :sparkles:

####Running a course with no tracking
When spoor is installed, main.html can be used instead of index.html to allow non-LMS use without any warning logs.

####Client Local Storage / Fake LMS / Adapt LMS Behaviour Testing
When spoor is installed, scorm_test_harness.html can be used instead of index.html to allow the browser to store LMS states inside a browser cookie. This allows developer to test LMS specified behaviour outside of an LMS environment.