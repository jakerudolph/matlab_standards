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
