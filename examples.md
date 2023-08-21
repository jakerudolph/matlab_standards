# Examples

Below are examples referred to by the standards.

## Primary Function Header

```matlab
 function dataslice = fetch_bsa_slice(timeRange, PVs, varargin)
% dataslice = fetch_bsa_slice(timeRange, PVs, varargin)
% ABSTRACT: Get all data from the BSA datastore for specified PVs in the 
% specified time window
%
% REMARKS: Uses the timeRange to find which directories to traverse,
% and then finds the specific files within the timeRange. Iterates through
% those files, pulling out data corresponding to the specified PVs and
% adding them to a datastruct.
% ARGUMENTS
%  timeRange: cell array of windowstart and windowend
%       -'windowstart': string of start time of window in format 
%                       'YYYY-mm-DD HH:MM:SS', in Pacific time
%       -'windowend': string of end time of window in format 
%                     'YYYY-mm-DD HH:MM:SS', in Pacific time
%  PVs: Cel array of PV strings or substrings, with data returned for
%         any PV found that contains one of the specified substrings
%
%
% OPTIONAL ARGUMENTS:
%  savefile: string of file to which to save the data, this will save in 
%              the same directory in which the script is run unless 
%              otherwise specified Note that the output files will be quite 
%              large, and could fail to save if disk space is insufficient
%  'sparseFactor': int defaulted to 1, if higher, will lower the frequency
%                  of data (i.e. sparsity of 2 will take every other data 
%                  point, sparsity of 3 every third, etc.)
%  'beamline': string of which beamline from which to pull data; 'CUH' for 
%              hard line, 'CUS' for soft line. Default will look at all PVs
%              and all files.
%  'batch': boolean defaulted to false, true will run data pull function 
%           with batch processing
%  'verbose': boolean defaulted to false, set to true to show progress 
%             messages (will not suppress memory estimate or num of files)
%
% OUTPUTS:
%  'dataslice': a struct containing the extracted data
%       -'ROOT_NAMES': cell array strings of the PVs read
%       -'the_matrix': a ROOT_NAMES x total points matrix of the extracted
%                      data.
%       -'isPV': a ROOT_NAMES x number of files logical matrix describing
%                which files contained data for which PVs
%
% EXAMPLE:
%
% data=fetch_bsa_slice({'2021-06-05 00:00:00','2021-06-05 12:00:00'},{'BPMS'},'beamline','CUH','batch',1)
%  -Returns data for all BPMS on the HXR line from June 5th 2021 at
%  midnight to June 5th 2021 at noon using batch processing.

% Compatibility: MATLAB 2020A and later
% Author: Jake Rudolph, SLAC, 2/9/23
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
% MOD:
%   3/24/23 - Updated header - Jake Rudolph
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
```
## Error Handling
All errors, both from MATLAB and in the functional execution of code, must be handled. In the context of GUIs, error handling should be used in all callback functions to prevent the GUI from ever crashing. Error handling should communicate both what happened when a problem was encountered, and what the code was in the middle of doing when the error occurred. In general, it is recommended to display high-level error information in a dialog box and print relevant detailed information to a log. This is the recommended paractice for GUIs as well as for functions and scripts called outside the context of a GUI. THe recommended error handling practices are as follows:

1. For GUIs, all callbacks should be wrapped in a try/catch loop.

2. In the catch section, check for specific types of errors related to your application. These errors should have a message code string, in the MATLAB style, using a prefix id specific to your app. You use the id in the callback function to identify your app's exceptions. This is important because your app's exceptions should not cause stacktraces in the log (the ugly and largely useless "crash" stuff). Only the others, ie MATLAB’s own exceptions, should cause such stacktraces. In that way, errors like "division by 0" get a stacktrace, useful for debugging, whereas errors like "Couldn't move wire scan motor" do not, since that's not a bug in MATLAB, it's just something it wasn't able to do but handled gracefully.

    a. If the exception is not one of the specific kinds your code looks for, the relevant information should be extracted from the stacktrace and communicated, including its location. Note, as a developer, you should pay attention to what kinds of errors are common, and do your best to encapsulate them with custom error IDs.

    b. Use uiwait(errordlg(lprintf())) in the catch block to communicate the error information to the log and in a dialogue box.

3. In the algorithmic function, when an error is detected, immediately (on the next line after you detected the error) issue a message using the Matlab "error" function.

4. The error() function's first argument should be the aforementioned message code string, and the second a more descriptive explanation. In every other (non-callback method), especially API methods, "throw exception." That is, whenever the program can't go on because of a functional problem, for example, you detect from EPICS that a wire is stuck, call error(). The method SHOULD also lprintf what happened.

5. In general, you should check for error status on all EPICS gets and puts, and implement appropriate error handling as described.

6. Do not catch and ignore errors, the user should always be informed of an error

Example of error handing pattern

[NOTE: constants below eg STDERR, WS_EXID_PREFIX etc, are defined in wirescan_const.m).

```matlab
try
    scanWireInit(hObject,handles,get(hObject,'Value'));
catch ex
    if ~strncmp(ex.identifier,WS_EXID_PREFIX,3)
        fprintf(STDERR, '%s\n', getReport(ex,'extended'));
    end
    uiwait(errordlg(... lprintf(STDERR, 'Problem changing wire. %s', ex.message)));
end
function handles = scanWireInit(hObject, handles, wireId)
…
    initFWS(handles); % API method can call other methods
    …
    function initFWS( handles )
        status = lcaPutSmart( pvName_reinit, initcode, 'short' );
        if ( status ~= LCA_SUCCESS )
            msgtext=lprintf(STDERR, 'Motor init failed', pvName_reinit );
            error('WS:MOTORINITFAILED', msgtext );
```

The message text SHOULD end in a period. The period is important because using the try catch scheme and using MATLAB’s builtin argument processing, messages are chained together.

An example of issuing an error() and printing to the log in one line. Note that the message says what is wrong AND what a user might then do.

```matlab
error('EM:NOGOODDATAREMAINS', lprintf(STDOUT,
... ['No data remaining that has both good measurement status'
... ' and user approved for use. Please include more scanned '
... ' device setpoints (Quad values or wires), or check "Use"'
... ' to allow inclusion of more in processing.']));
```
