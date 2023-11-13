# MATLAB Standards
*These standards apply to all newly written or newly modified code. It is not expected that existing code will be retrofitted to conform with these standards.*

**Status:** This document is in development as of July 17th, 2023

**Scope:** This document details source code requirements and programming idioms required for all MATLAB code of the Accelerator Directorate, SLAC National Accelerator Laboratory. It additionally gives some recommended practices and examples.

**Purpose:** The purpose of this document is to provide consistent guidelines for producing robust, readable code that is more easily maintained and understood by current and future developers.

**Use of Normative Statements:**

  * **MUST,** in all capitalized letters, indicates a mandatory standard.
  * **SHOULD,** in all capitalized letters, indicates a recommended standard.
  * **MAY,** in all capitalized letters, indicates a recommended, but optional practice.


# Naming Conventions


## Name Shadowing

**Description:** Functions and variables **MUST NOT** shadow Mathworks-shipped functionality or other code. This includes MATLAB files (.m, .p, .mlapp, .mlx), mex-files (.mexw32 etc.) and Simulink files (.mdl, .slx).

**Rationale:** Shadowing code can result in unexpected behavior, because it is unclear and not properly defined for example what function is called when multiple ones share the same name.

## Units

**Description:** Variable units **MUST** be documented, either in the variable name, or as end-of-line comments in square brackets. Prefer end-of-line comments if addition to the variable name will make it excessively long.

**Rationale:** Maintain readability by using concise variable names.

DO:
```matlab
time_min = 2; 
time_s = 120; 
acceleration = 1000; % [mm/s^2] 
```

DON'T:
```matlab
time = 2; 
accelerationInMillimetersPerSecondSquared = 1000;
```
		
## Negated Boolean Names

**Description:** Variable names **SHOULD NOT** include the word not.

**Rationale:** Readability decreases when variables have negated boolean names, especially in combination with the ~ operator.

DO:
```matlab
if isValid && ~isFound 
    error('My error message.') 
end 
```

DON'T:
```matlab
if ~notValid && isNotFound 
    error('My error message.') 
end 
```

## Descriptive Names

**Description:** Names for functions, classes, packages and variables **MUST** be precise and descriptive.

**Rationale:** This increases the readability of the code.

DO:
```matlab
function degreesK = convertCelsiusToKelvin(degreesC)
```

DON'T:
```matlab
function dk = convC2k(degrees) 
```
	
## The 'n' Prefix

**Description:** The prefix n **SHOULD** be used for variables that represent a number of things. The prefix **SHOULD NOT** be used for other purposes, and other prefixes for representing the number of things **SHOULD NOT** be used.

**Rationale:** Variables starting with this prefix can easily be identified.

DO:
```matlab
nFiles 
nCars 
nLines 
```

DON'T:
```matlab
numberOfFiles 
nrCars 
lineCount
```

## Name Length

**Description:** Variable names **should** be at most 32 characters long. Function names **should** be at least 3 and at most 32 characters long.

**Rationale:** Names shall be descriptive, so should not be too short. However, variable names can sometimes be single characters (x, y) and still be descriptive. Names that are too long can reduce readability because they introduce long lines of code.

DO:
```matlab
maxData = max(data);     
```

DON'T
```matlab
maximumValueOfTheEntireSetOfDataPoints = max(data);
```

## Loop Iterator Naming

**Description:** Iterator variable names **SHOULD** be at least 3 characters long. In nested for-loops, the starting letters of the iterator names **SHOULD** be subsequent letters in the alphabet.

**Rationale:** Iterator variables of for-loops are easily distinguishable from other variables by using these prefixes.

DO:
```matlab
for iNode = 1 : 10 
    for jPoint = 1 : 3 
       	myGraph(iNode).plotIt(jPoint); 
    end 
end 
```

DON'T:
```matlab
for nodeIdx = 1 : 10 
    for j = 1 : 3 
       	myGraph(nodeIdx).plotIt(j); 
    end 
end   
```

## Parent / Child Name Redundancy

**Description:** Names of fields and properties **SHOULD NOT** contain their parent struct or object's name.

**Rationale:** When referenced, the fields and properties can have the same phrase multiple times in a row, which is unnecessary.

DO:
```matlab
Beam.thickness 
chair.weight 
```

DON'T:
```matlab
Beam.beamThickness 
chair.weightOfChair
```

## Names to Avoid

**Description:** Names of classes, functions, variables, properties and struct fields **MUST NOT** start with generic names (test, my, the) as listed below and they **MUST NOT** have names such as myClass, testFunction, etc. The only exception is a unit test method or file, which may incorporate the word test.

**Rationale:** Names like these are unhelpful and do not properly indicate what the class, function or variable is for.

In particular, do not use the following (or similar names): 
```matlab
myClass 
my_Class 
TheClass 
myFunction 
myFunc 
my_function 
TheFunc 
myVar 
my_var 
theVar 
myProp 
my_prop 
test
tst
testing 
```

## Function Names Document Their Use

**Description:** The names of functions **MUST** document their specific use.

**Rationale:** By choosing a clear and descriptive name, the code becomes more readable.

DO:
```matlab
model_rMatGet 
control_bpmGet 
util_gaussFit 
gui_sliderControl  
```

DON'T:
```matlab
modelFunction
doTheThing
MeasureStuff
```

## Function Name Construction

**Description:** Function names **SHOULD** consist of a verb (action) followed by a noun (direct object). For class methods, the direct object can be omitted if it is redundant. 

**Rationale:** This way it is more likely to be clear what the function does and if there is no suitable name following this guideline, (for example because the function does more than one thing) the function may have to be refactored.

DO:
```matlab
getPosition
findOutlier
multiplyNumbers
updateModel
getTwiss
standardizeMagnet
checkConvergence
```

DON'T:
```matlab
rMat 
gauss 
bpms 
sliderstuff 
```

## Casing (General)

**Description:** Variables, functions, classes, and other MATLAB objects **MUST** each use a consistent casing style throughout a file.

**Rationale:** Consistent casing increases readability and maintainability.

**Types of Casing:**

	* b (single lowercase letter) 
	* B (single uppercase letter) 
	* lowercase 
	* lower_case_with_underscores 
	* UPPERCASE 
	* UPPER_CASE_WITH_UNDERSCORES 
	* CapitalizedWords (or CapWords, or CamelCase ) 
	* Acryonyms should retain caps 
	* mixedCase (differs from CapitalizedWords by initial lowercase character!) 
	* Capitalized_Words_With_Underscores 

## Constant Casing

**Description:** Constant variable names **MUST** be written in all-caps with underscores separating the words (UPPER_CASE_WITH_UNDERSCORES). 

**Rationale:** Enforcing this kind of casing easily distinguishes constants from actual variables.

## The 'is' Prefix

**Description:** The is prefix **SHOULD** be used for functions returning logical values or properties and variables holding logical values.

**Rationale:** This increases the readability of the code as it is clear from the name, and avoids unexpected code behavior.


## Abbreviations

**Description:** Variable names **SHOULD NOT** contain abbreviations; except for idiomatic abbreviations commonly used at SLAC.

**Rationale:** Maintain readability by not abbreviating words in your variable names. Common acronyms are allowed.

DO:
```matlab
arrivalTime 
bpmCharge 
```
 
COMMON EXCEPTIONS: 
```matlab
bpm 
toro 
xcor 
ycor 
bdes
rf_phase
sc_linac
ltuh_xcor
```
	
DON'T:
```matlab
arrTim 
bpmC
phcvty
```	
	
## The 'Test' Suffix

**Description:** The suffix "Test" or "test" **SHOULD** only be used for unit test filenames.

**Rationale:** This increases the readability of the code as it is clear from the name of the file that it is a unit test file.

# Layouts and Comments

## Comment Usage

**Description:** All code **MUST** use comments to explain code functionality. Comments should explain what the code does, how it works, and why it is performing this function.

**Rationale:** Descriptive comments make it easier to read code and figure out how the code works. 

## Comment Content

**Description:** Comments **MUST NOT** use the same terminology as the code. Use different words for describing what a particular object is, or for what a piece of code does. 

**Rationale:** Comments that simply restate the code in plain English do not help explain what the code is doing. 

DO:
```matlab
BPM_x = 12; % horizontal position 
 
% find where the horizontal difference orbit is within defined range by iterating through  
% the difference orbit array by BPM, and comparing the to the acceptance difference of 0.1  
% in order to see where the orbit deviates horizontally. 
in_range = false(nBPMS, 1); 
for iBPM = 1:nBPMS 
    if diff_orbit_x(iBPM) < 0.1 
	is_in_range(iBPM) = True; 
    end 
end 
```

DON'T:
```matlab
BPM_x = 12; % set x BPM coordinate to 12
	 
% find x BPMs in range 
in_range = false(nBPMS, 1); 
for iBPM = 1:nBPMS 
    if diff_orbit_x(iBPM) < 0.1 
	Is_in_range(iBPM) = True; 
    end 
end 
```

## Comment Frequency

**Description:** Comments **SHOULD** be used at least every 15 lines. 

**Rationale:** This general rule helps to avoid going too long without commenting code. 

## Sign and Date Comments

**Description:** Comments with a date and signature **MAY** be used when adding code to an existing file.

**Rationale:** This ensures that other code maintainers have access to information about the code history regardless of how version control is deployed, and can easily contct the last person who modified the code if there are conflicts/question. 

## Comment Blocks

**Description:** Comment blocks **SHOULD NOT** be used.

**Rationale:** Consistent comment syntax improves readability.

DO:
```matlab
% This is a comment. 
% This is also a comment. 
```

DON'T:
```matlab
%{ This is a comment. 
This is also a comment. 
%}
```

## Function Indentation

**Description:** Functions **SHOULD** be indented on the root level of a file.

**Rationale:** Consistent indentation improves readability.

DO: 
```matlab
function computeWeight(in) 
    weight = 2 * in; 
end 
```

DON'T:
```matlab
function computeWeight(in) 
weight = 2 * in; 
end 
```

 ## Whitespace in Statements

 **Description:** The following operators **SHOULD** be surrounded with spaces: = : ^ == <= >= < > ~= & &&  | ||  + - * .* /  A space **SHOULD NOT** be used between the unary minus-sign and its operand. 

**Rationale:** Whitespace around operators often leads to improved readability. As an exception, do not use spaces around the equals signs when using the Name=Value syntax introduced in MATLAB R2021a. 

DO:
```matlab
isValid = (y > 0) && (x == -1); 
```
 
DON'T:
```matlab
isValid=(y>0)&&(x==-1);
```

## Whitespace in Characters

**Description:** Commas, semicolons and keywords (for, while, if etc.) **SHOULD** be followed by a space unless at the end of the statement. 

**Rationale:** A clear separation of keywords from other code improves readability. 

DO:
```matlab
while ismember(x, [3, 5, 7, 11, 13]) 
    x = x + 2; 
end
```
 
DON'T
```matlab
while ismember(x,[3,5,7,11,13])
    x = x + 2; 
end
 ```

## Line Length

**Description:** Lines **SHOULD** be no more than 120 characters long, including comments. When possible, keep lines shorter than 80 characters long. 

**Rationale:** A line of code should fit on a screen. Long lines of code are difficult to interpret and generally indicate excessive complexity.

DO:
```matlab
theText = ['Lorem ipsum dolor sit amet, consectetur adipiscing elit. ', ... 
	   'Ut luctus arcu ex, at viverra nibh malesuada ac. Proin vel arcu leo.']; 
```

DON'T
```matlab
theText = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Ut luctus arcu ex, at viverra nibh malesuada ac. Proin vel arcu leo.';
```

## Indentation Length

**Description:** Indentation **SHOULD** be four spaces long. This is the default setting in the MATLAB editor. 

**Rationale:** Using an adequate and consistent number of spaces for indentation improves readability and prevents unnecessary changes when others work on the code. 

DO:
```matlab
if x > 0 
    if x < 2 
        disp(x) 
    end 
end 
```

DON'T
```matlab
if x > 0 
 if x < 2 
   disp(x) 
 end 
end
```

## Comments on Poorly Written Code

**Description:** Comments **SHOULD NOT** be used to justify or mock poorly written code. Write proper code that requires no justification instead. 

**Rationale:** Readability will be improved if issues with the code are fixed immediately instead of documented in the comments. 

DO:
```matlab
x = 2; 
y = 3 * x + z;  
	 
z = [1, 23, 8.1] * (x ^ 3.1415) * sqrt(y); 
```

DON'T
```matlab
x = 2; 
	 
% FIXME: This piece of ugly code is copied from StackExchange. 
z=[1,23,8.1]*(x^3.1415)*sqrt(y);
```

## Commas in Rows

**Description:** Commas **MUST** be used to separate elements in a row. Place the comma directly after the element. 

**Rationale:** This improves readability, especially in arrays of strings or character vectors. 

DO: 
```matlab
fig.Position    = [0, 1000, 1.5, 130]; 
saying          = ['An apple a ', period, ' keeps the doctor away.']; 
```
 
DON'T: 
```matlab
fig.Position    = [0 1000 1.5 130]; 
saying          = ['An apple a ' period ' keeps the doctor away.']; 
```

## Line Continuation with Operators

**Description:** Binary operators **SHOULD** be placed in multi-line statements at the start and not at the end of the line. 

**Rationale:** By placing binary operators at the start of the line, it is immediately clear what the line does. 

DO: 
```matlab
isDone = isImplemented ... 
    && isTested ... 
    && isDocumented; 
```
     
DON'T: 
```matlab
isDone = isImplemented && ... 
    isTested && ... 
    isDocumented; 
```

## Methods/Properties Blocks with Duplicate Attributes

**Description:** Multiple properties and methods blocks with equal attribute values **SHOULD** be combined into one. Do not set attributes to their default values. 

**Rationale:** Having multiple blocks with the same attributes is unnecessary and decreases the readability of code. 

DO
```matlab
classdef Rocket 
    properties (Constant) 
        Fuel = "Nuclear" 
        Capacity = 2 
    end 
end 
```

DON'T
```matlab
classdef Rocket 
    properties (Access = public, Hidden = false, Constant) 
        Fuel = "Nuclear" 
    end 
	     
    properties (Constant = true) 
        Capacity = 2 
    end 
end
```
 
## Whitespace at the End of a Line

**Description:** White space **SHOULD NOT** be used at the end of a line of code unless it is followed by a comment. 

**Rationale:** This prevents smart-indenting a file from inducing changes to those lines. 

## Help Text

**Description:** Header comments **SHOULD** be written with text markup to provide user documentation useful both in-file and when using the help and lookfor functions.

**Rationale:** This increases readability and makes the code more likely to be found and less likely to be reinvented. 

DO:

[Header Example](./examples.md#Primary-Function-Header)

DON'T:
```matlab
function [avg, maxSize] = measureSize(obj, dim) 
    avg = mean(obj.Size, dim); 
    maxSize = max(obj.Size, dim); 
end
```

## Whitespace Around Blocks of Code

**Description:** Coherent blocks of code **SHOULD** be separated by one or more blank lines. 

**Rationale:** Increases the readability of the code. 

DO:
```matlab
% Shift to origin. 
deltaX = mean(xIn); 
xOutc = xIn - deltaX; 
	 
% Calculate scaling. 
scalingFactor = newZ / currentZ; 
xOut = xOut .* scalingFactor; 
	 
% Shift back. 
xOut = xOut + deltaX; 
```

DON'T
```matlab
deltaX = mean(xIn); 
xOut = xIn - deltaX; 
scalingFactor = newZ / currentZ; 
xOut = xOut .* scalingFactor; 
xOut = xOut + deltaX;
```

## Correct Spelling

**Description:** Correct spelling and punctuation **SHOULD** be used in comments. Write in complete sentences, starting with a capital letter and ending in a period. 

**Rationale:** This increases the readability of code. Error-strewn and poorly constructed comments reflect on the code they accompany. 

DO:
```matlab
% Get the scaling factor. 
scalingFactor = newZ / currentZ; 
xOut = xOut * scalingFactor; % Now rescaled.  
```

DON'T:
```matlab
% get sclaing factor 
scaling_factor = new_z / current_z; 
x_out = x_out * scaling_factor; % now recslaed
```

## Align Struct Definition

**Description:** When defining a struct, the names and values of the struct fields **SHOULD** be aligned. Each field **MAY** be commented. 

**Rationale:** This improves readability of the code by making it clearer what fields are defined and by what values. 

DO:
```matlab
s = struct(... 
    "name",     "Albert", ... % String, first name of physicist
    "age",      73, ... % Integer, age of physicist in years
    "isValid",  ismember("Albert", allNames)) % Logical, represents if actively employed
```
     
DON'T:
```matlab
s = struct("name", "Albert", "age", 2021 - 1979, "isValid", ismember("Albert", allNames)); 
```

## End Keyword

**Description:** Functions **MUST** conclude with the "end" keyword.

**Rationale:** Using the "end" keyword makes it easier to match indentations in code organization. 

# Statements and Expressions

## Constant Definitions

**Description:** Constant definitions **SHOULD** be placed at the top of the function, after defining global/persistent variables. For classes, constant definitions **SHOULD** exist in a separate file.

**Rationale:** Consistent constant definition allows for easy identification.

DO:
```matlab
function testConstants(x) 
    MAX_Z = 100; 
		 
    y = 3 * x; 
    z = min(y, MAX_Z); 
end 
```
 
DON'T:
```matlab
function testConstants(x) 
    y = 3 * x; 
    MAX_Z = 100; 
    z = min(y, MAX_Z); 
end
```

## Variable Declarations

**Description:** Array variables MUST be pre-allocated to the expected total size if elements will be added/modified individually. All variables MAY be declared at the top of a function for readability.

**Rationale:** Pre-allocating array sizes instead of repeatedly resizing an array improves code performance and memory management. It is easier to find and modify variable in general if they are at the top of a function.

DO:
```matlab
function [var1, var2, var3, finalvar] = get_vars() 
    var1 = 10 ; 
    var2 = var1^2;
    var3 = var2 * 12; 
    finalvar = zeros(1, 10000);
    for iElem = 2:10000
        finalvar(iElem) = finalvar(iElem-1) + 2;
    end
end
```

DON'T:
```matlab
function [var1, var2, var3, finalvar] = get_vars() 
    finalvar = 0;
    for iElem = 2:10000
        finalvar(iElem) = finalvar(iElem-1) + 2;
    end
    var1 = 10 ; 
    do_some_other_thing;
    check_device_status;
    var2 = var1^2;
    disp(var1);
    var3 = var2 * 12; 
end
```

## Magic Numbers

**Description:** Magic numbers **SHOULD NOT** be used in expressions. Define them as constants before using them. 

**Rationale:** This encourages reuse and documentation of numerical constants and improves overall readability.

DO:
```matlab
DECK_SIZE   = 52; 
N_VALUES    = 13; 
 
for iPick = 1 : DECK_SIZE 
    disp(randi(N_VALUES)); 
end 
```

DON'T:
```matlab
for iPick = 1 : 52 
    disp(randi(13)); 
end 
```

## Function Calls Without Inputs

**Description:** Empty parentheses **MUST** be used for function calls without input arguments. 

**Rationale:** This helps to emphasize the fact that they are function calls and not variables or properties. 

DO:
```matlab
x = rand() * 2; 
```

DON'T:
```matlab
x = rand * 2;
```

## One Statement Per Line

**Description:** Each line **SHOULD** have at most one statement.

**Rationale:** Maintain readability of your code by having each line of code do exactly one thing. 

**Exception:** One can declare a maximum of three variables on a single line if they are all of the same type or unit. One can place a simple if/else statement or ternary operation on one line. A for or while statement can be placed on one line if there is exactly one statement inside of the loop. 

DO:
```matlab
for iNode = 1 : 3 
    x = x + iNode; 
    disp(x); 
end 
	 
a = 1;  % Unitless 
b = 2;  % Unitless 
c = 3; % Unitless 
disp(a + b + c); 
	 
time_limit = 2; % [minutes] 
time_start = 4; % [seconds] 
time_stop = 9; % [seconds] 
time_elapsed = time_stop - time_start; 
```

DON'T:
```matlab
for iNode = 1 : 3, x = x + iNode; disp(x); end 
	 
a = 1;  % Unitless 
b = 2;  % Unitless 
c = 3; % Unitless 
disp(a + b + c); 
	 
time_limit = 2; % [minutes] 
time_start = 4; % [seconds] 
time_stop = 9; % [seconds] 
time_elapsed = time_stop - time_start; 
```

EXCEPTIONS:
```matlab
% Variable declarations:
persistent x, y; 
	 
[a, b, c] = deal(1, 2, 3); 
disp(a + b + c); 
 
time_limit = 2; % [minutes] 
[time_start, time_stop] = deal(4, 9); % [seconds] 
time_elapsed = time_stop - time_start; 
	 
% One line loop: 
for iNode = 1 : 3, x = x + iNode; end  % 1 statement inside loop is allowed

% One line if/else (or equivalent ternary operation) with only one statement per branch:
if (a > b), b = 0; else, b = 1; end
```
 
## If/Else

**Description:** Every if **SHOULD** have an else section, even if it does not contain executable code. 

**Rationale:** By including an else section, no execution paths are overlooked. Additionally, code coverage can be accurately reported because without else, it is unclear whether the if-condition is ever false. 

DO:
```matlab
if x > 0 
    saveData() 
else 
    % x does not indicate we want to save, so do nothing  
end 
```

DON'T:
```matlab
if x > 0 
    saveData() 
end
```

## Switch Otherwise:

**Description:** Every switch **SHOULD** have an otherwise section, even if it does not contain executable code. 

**Rationale:** By including an otherwise section, no execution paths are overlooked. Additionally, code coverage can be accurately reported because without otherwise, it is unclear whether it happens that none of the cases occur. 

DO:
```matlab
switch reply 
    case "Yes" 
        saveData() 
    case "No" 
        clearData() 
    otherwise 
        % Should not get here. 
        error("Unknown reply: " + reply) 
end 
```

DON'T:
```matlab
switch reply 
    case "Yes" 
        saveData() 
    case "No" 
        clearData() 
end
```

## Mixed Types in Expressions

**Description:** Multiple operand types **SHOULD NOT** be used in a single operator. For example, do not mix logical and numerical operands with a multiplication operator. 

**Rationale:** Mixing operand types per operator can cause unexpected results and can lead to errors in case of true incompatibilities. 

DO:
```matlab
d = (a * b > 0) && c; 
```
 
DON'T:
```matlab
d = a * b && c;
```

## Parentheses in Logical Expressions

**Description:** Parentheses **SHOULD** be used to clarify the precedence of operands in expressions with multiple logical operators. No parentheses are required when only one type of logical operator is used. For example: d = a && b && c;. 

**Rationale:** By clarifying the precedence of the operands in such expressions, readability is improved and unexpected behavior is prevented. 

DO:
```matlab
d = (a && b) || c; 
```
 
DON'T:
```matlab
d = a && b || c;
```

## Parentheses in Mathematical Expressions

**Description:** Parentheses **SHOULD** be used to clarify the precedence of operands in expressions with multiple mathematical operators. No parentheses are required when only one type of mathematical operator is used. For example: d = a * b * c;. 

**Rationale:** By clarifying the precedence of the operands in such expressions, readability is improved and unexpected behavior is prevented. 

DO:
```matlab
d = (a * b) + c; 
h = (f / 2) * g; 
```

DON'T:
```matlab
d = a * b + c; 
h = f / 2 * g;
```

## Assert Inputs

**Description:** Every call to the built-in assert function **MUST** have an error identifier and a message as second and third inputs. 

**Rationale:** Including an identifier allows for better testing and exception handling. Including a proper error message directly clarifies the problem. 

DO:
```matlab
assert(~isempty(list), 'ClassName:MethodName:ListIsEmpty', 'The list shall not be empty.') 
```

DON'T:
```matlab
assert(~isempty(list))
```

## Global Variables

**Description:** The global keyword **SHOULD NOT** not be used. 

**Rationale:** The value of global variables can change from anywhere, which can make things unnecessarily complicated.

## Constant Conditional Statements

**Description:** Conditions that are always true or always false, such as if true or elseif 0, **SHOULD NOT** be used. MATLAB's Code Analyzer warns about some of these cases. 

**Rationale:** These constructs can cause unreachable code or unintended behavior. 

DO:
```matlab
disp(x)
```

DON'T:
```matlab
if x > 0 || true 
    disp(x) 
end    
```

## Struct for Related Entities

**Description:** A struct **SHOULD** be used to group together related entities. 

**Rationale:** Using a struct for related entities improves code organization. 

DO:
```matlab
Bpm.x 
Bpm.y 
Bpm.tmit 
```

DON'T:
```matlab
Bpm_x 
Bpm_y 
Bpm_tmit 
```

## Group Struct Field Definitions

**Description:** All fields of a struct **SHOULD** be defined in a single, contiguous block of code. No fields should be added to a struct after using it. 

**Rationale:** Lines of code or functions using the struct might assume that all fields are already there, resulting in an error if the field is not there. 

DO:
```matlab
s = struct(...
    "f", 2, ...
    "g", 3, ...
    "h", 'new field');

computeCost(s); 
```

DON'T:
```matlab
s = struct("f", 2, "g", 3); 
	 
computeCost(s); 
s.h = 'new field'; 
```
 
## Eval Functions

**Description:** The built-in eval function **SHOULD NOT** be used. Use of feval, evalin and evalc **SHOULD** be minimized.

**Rationale:** These functions can have harmful results and statements using them are usually difficult to read and maintain. 
Additionally, it can be overlooked when variables are defined or altered by calls to these functions.

## Keywords Break, Continue, and Return

**Description:** Use of the keywords break, continue and return **SHOULD** be avoided. 

**Rationale:** Using break, continue or return decreases readability because the end of the function/loop is not always reached. By avoiding these keywords, the flow of the code remains clear. However, these keywords can occasionally be useful to reduce complexity and improve performance, hence a recommendation level of should applies and they may be used (with developer justification) in certain situations. 

DO:
```matlab
if isempty(x) 
    % No further actions. 
else 
    <rest of the code> 
end

for iNode = 1:10
   if (iNode == 3)
      % no further action
   else
      <rest of code>
   end
end

```

DON'T:
```matlab
if isempty(x) 
    return 
end
<rest of the code>

for iNode = 1:10
   if (iNode == 3)
      continue
   end
   <rest of code>
end
```

## Shell Escape

**Description:** The shell escape function **SHOULD NOT** be used. If necessary, use the system function instead. 

**Rationale:** When using the !program_to_execute syntax to execute external programs, no dynamic input or program names can be used. 

DO:
```matlab
system('mycommand') 
```
 
DON'T:
```matlab
!mycommand 
```

## Try/For Exception Handling

**Description:** The try construct **SHOULD** only be used for exception handling. An exception object must be assigned or created and an error function must be called in the catch. Do not use try/catch to suppress errors or to express simple conditions. There are other, more suitable options for that. 

**Rationale:** Using try for simple error suppression can result in poor performance and unexpected behavior. When a different error occurs than the one expected, it can go undetected because of the try/catch. 

DO:
```matlab
if isfield(container, 'nElements') 
    nElems = container.nElements; 
else 
    nElems = 0; 
end 
```
 
DON'T:
```matlab
try 
    nElems = container.nElements; 
catch 
    nElems = 0; 
end
```

## Floating-Point Comparisons

**Description:** Floating-point values **SHOULD NOT** be compared using == or ~=. Use a tolerance instead. 

**Rationale:** Rounding errors due to algorithm design or machine precision can cause unexpected inequalities. 

DO:
```matlab
out = abs(myFcn(in) - sqrt(3)) < 1e-12; 
```

DON'T:
```matlab
out = myFcn(in) == sqrt(3);
```

## Workspace Variable Ans

**Description:** The workspace variable ans **SHOULD NOT** be used.

**Rationale:** Use of the workspace variable ans reduces the robustness and security of the code. 

## Debugging Functions

**Description:** Debugging functions such as dbstep, dbcont, dbquit, keyboardor other functions that interact with the MATLAB debugger **SHOULD NOT** be used within a script. 

**Rationale:** These functions that interact with the MATLAB debugger reduce the robustness of the code. 

## Logical Indexing

**Description:** Logical indexing **SHOULD** be used instead of linear or subscript indexing where possible.

**Rationale:** Logical indexing is faster and more readable. 

DO:
```matlab
index = (v > MIN) & (v < MAX); 
```
 
DON'T
```matlab
index = intersect(find(v > MIN), find(v < MAX));
```

## Loop Vectorization

**Description:** Vectorization **SHOULD** be used to apply operations to arrays where feasible.

**Rationale:** Vectorized loops are faster, more robust, more readable, and more maintainable. Matlab was designed to work in this way. 

DO:
```matlab
dataResult = dataset < limit; 
 ```

DON'T:
```matlab
dataResult = false(size(dataset)); 
	 
for iElem = 1 : numel(dataset) 
    if dataset(iElem) < limit(iElem) 
        dataResult(iElem) = true; 
    else 
        dataResult(iElem) = false; 
    end 
end
```

## Zero Before Decimal Point

**Description:** A zero before the decimal point **MUST** be used in numeric expressions. 

**Rationale:** Increases the readability of the code. 

DO:
```matlab
THRESHOLD = 0.5;
```

DON'T:
```matlab
THRESHOLD = .5;
```

## Accessing Other Workspaces

**Description:** Functions accessing workspaces outside the scope of the current function, such as assignin, evalin, clc, **SHOULD NOT** be used within scripts. 

**Rationale:** These functions reduce the portability, robustness and readability of the code. 

## Use Switch Blocks

**Description:** A switch block **SHOULD** be used instead of many if-elseif-else statements when possible. 

**Rationale:** The notation of a switch block is more concise and therefore, more readable and easier to maintain. 

DO:
```matlab
switch name 
    case "Bill" 
        ... 
    case {"Judith", "Bob"} 
        ... 
    case "Steve" 
        ... 
    otherwise 
        ... 
end 
```

DON'T:
```matlab
if name == "Bill" 
    ... 
elseif ismember(name, ["Judith", "Bob"]) 
    ... 
elseif name == "Steve" 
    ... 
else 
    ... 
end
```

# High Level Guidelines

## Complexity

**Description:** The number of lines of code and complexity of functions **SHOULD** be minimized as appropriate, especially for nested functions and methods in a classdef file. See standard on Single Responsibility below.

**Rationale:** Short functions are more likely to be readable, reusable and testable. 

## Single Responsibility

**Description:** Single responsibility principle : all sub-functions and most functions **SHOULD** do one thing very well, and encapsulate a single idea. 

**Rationale:** The single responsibility principle makes code more readable, modular, and testable. Multi-purpose functions can often be split up into atomic units which are called on the input data in sequence. Functions should not try to be all things to all users. 

## Number of Function Arguments

**Description:** The number of inputs and outputs of a function **SHOULD** be limited to five. If necessary, combine multiple arguments into a struct. 

**Rationale:** This helps prevent a long calling syntax, which in turn improves the readability and reusability of the code. 

DO:
```matlab
out = computeWeight(blockData, nBlocks, idx); 
```
 
DON'T:
```matlab
out = computeWeight(blockHeight, blockWidth, blockDepth, density, nBlocks, idx);
```

## Constructors with Single Output

**Description:** Every constructor **MUST** have exactly one output: an object of the class. 

**Rationale:** A second constructor output is unusual, so will be unexpected for readers of the code. 

DO:
```matlab
classdef Computer 
    methods 
        function obj = Computer(in) 
            obj.value = in; 
        end 
    end 
end 
```

DON'T:
```matlab
classdef Computer 
    methods 
        function [obj, out2] = Computer(in) 
            obj.value = in; 
            out2 = 2 * in; 
        end 
    end 
end
```

## Scripts

**Description:** Functions **SHOULD** be used instead of scripts, even if there are no inputs or outputs. 

**Rationale:** The scope of variables in a script is not bounded as one might expect. It is therefore safer to use functions instead. Additionally, scripts are not compatible with Matlab Coder. 

## Editor Warnings

**Description:** Code Analyzer messages shown in the MATLAB editor **SHOULD** be prevented or suppressed. When the messages cannot be prevented, suppress them on an individual basis and add the reason in the comments. 

**Rationale:** The messages usually indicate that improvements can be made to the performance or stability of the code. Adding file-wide Code Analyzer suppressions is discouraged because new triggers of the messages can be overlooked. 

DO:
```matlab
%#ok<ABCDE> Suppress the Code Analyzer warning on this line alone. 
```
 
DON'T:
```matlab
%#ok<*ABCDE> Suppress a Code Analyzer warning in the entire file.
```

## Getters and Setters

**Description:** Dependent properties **SHOULD NOT** be used. Write explicit methods for getting or setting property values if necessary instead of get.property and set.property syntax.

**Rationale:** Property getters and setters can cause unexpected behavior because code is run unexpectedly when the property's value is changed or requested. 

## Nesting Depth

**Description:** Blocks of code **MUST NOT** be nested deeper than ten levels, and **SHOULD NOT** be nested deeper than five levels. If necessary, refactor into multiple functions.

**Rationale:** Deeply nested code is often less readable and it can be a sign of inefficient code. 

## Repeated Code

**Description:** If a function contains a block of code that is repeated more than one time, it **SHOULD** be refactored into a single block of code. 

**Rationale:** Refactoring of repeated blocks of code increases the maintainability, readability and reusability of the code. 

## Structure Ownership

**Description:** Ownership of a structure resides with the creator of that structure, so fields **SHOULD NOT** be added or removed from an existing structure outside of the function in which it was created.

**Rationale:** Adding or removing fields from an existing structure outside of the function in which it was created reduces the robustness of the code. Related to coder compatibility, in generated code, adding a new field to a structure that has already been read or indexed will cause an error. If there is a need to modify a structure created elsewhere, consider reformatting the code to convert the structure to a class.

DO:
```matlab
function [out] = calculateSpeed(car) 
	 
out = car; 
	 
% clear the field 
car.speed = []; 
```

DON'T:
```matlab
function [out] = calculateSpeed(car) 
	 
out = car; 
	 
% do not remove fields from existing struct 
car = rmfield(car, 'speed');
```

## Commented Out Code

**Description:** Commented-out lines of code **SHOULD** be removed.

**Rationale:** Removing commented-out code increases readability and it prevents someone from unintentionally enabling it again. 

## Reuse of Iterator Variables

**Description:** Iterator variables **SHOULD NOT** be reused within the same function. Limit the scope of an iterator variable to its for-loop. 

**Rationale:** Prevents renaming all iterators when only the one for a specific loop must be renamed. Also improves readability. 

DO:
```matlab
for iNode = 1 : numel(nodes) 
    ... 
end 
	 
for iValue = 1 : 2 : 11 
    ... 
end 
```

DON'T:
```matlab
for iNode = 1 : numel(nodes) 
    ... 
end 
	 
for iNode = 1 : 2 : 11 
    ... 
end
```

## Rewriting Existing Functions

**Description:** An existing function **SHOULD NOT** be copy-pasted as a local function of your code. Instead, use a wrapper around existing functionality. 

**Rationale:** Multiple forks, duplications and reinventions of the wheel make a code base less modular and less maintainable. 

## Columns with Implicit Meaning

**Description:** Matrices in which rows or columns have implicit meaning **SHOULD NOT** be used. Use individual vectors instead. 

**Rationale:** Using matrices with implicit row or column meanings reduces the readability and robustness of the code. 

DO:
```matlab
distance = sqrt(x .^ 2 + y .^ 2); 
```
 
DON'T:
```matlab
distance = sqrt(coordinates(:, 1) .^ 2 + coordinates(:, 2) .^ 2); 
```

## Nested Functions

**Description:** Use of nested functions **SHOULD** be minimized. Use only when really beneficial. 

**Rationale:** Nested functions reduce the readability and robustness of the code and can not be tested independently. 

## Input Parameter Dependency

**Description:** Function input arguments **SHOULD** be independent, and the function **SHOULD** not assume a dependency between them. 

**Rationale:** Assuming a dependency between independent input parameters reduces the robustness of the code. Conversely, if the inputs to a function are dependent, treating them as independent also reduces robustness. 

DO:
```matlab
function [meanV, meanA] = getMeanVandA(distances, timePts) 
    meanV      = sum(distances) ./ sum(timePts); 
    velocities = distances ./ timePts; 
    meanA      = velocities(end) ./ sum(timePts); 
end 
	 
function vectorOut = processVector(vectorIn) 
    for iIdx = 1 : length(vectorIn) 
        vectorOut(ii) = processScalar(vectorIn(ii));
    end 
end 
```

DON'T:
```matlab
function [meanV, meanA] = getMeanVandA(velocities, distances, timePts) 
    meanV = sum(distances) ./ sum(timePts);
    meanA = velocities(end) ./ sum(timePts); 
end 
	 
function vectorOut = processVector(vectorIn, vectorLength) 
    for iIdx = 1 : vectorLength 
        vectorOut(ii) = processScalar(vectorIn(ii)); 
    end 
end
```

## Java Dependency

**Description:** Code **SHOULD** be independent of Java and therefore **SHOULD NOT** have Java dependencies, dependencies such as: javacomponent, javaMethod, javaObjectEDTetc. 

**Rationale:** Java packages and subpackages will not be available in MATLAB in a future release.

# Standards for Graphical User Interfaces (GUIs)

## Separable Models

**Description:** Model classes **SHOULD** be able to run without the GUI itself. Consider the separate aspects of a model, including service logic, service configuration, and application state management.

**Rationale:** Separable models allow for service logic to be run without the overhead of the GUI, increasing the usability of code across multiple platforms.

## Services and Persistent Global Processes

**Description:** Services, models, servers or other processes that run persistently to provide functionality for multiple clients **SHOULD** have stop, start, restart and view log buttons along with heartbeat and process running indicators. These buttons **SHOULD** be placed on a watcher panel.

**Rationale:** Any important global service needs to be accessible and transparent to all, with the ability to determine if said service is running and attempt to restart it if it is not running.

## Resizing

**Description:** GUIs **MUST** be resizable for personal computers. Consider using grid layouts to ensure resizability.

**Rationale:** The size of a GUI can vary between an individual's personal computer, and an operator interface in the control room. GUIs should be usable on both.

## GUI Documentation

**Description:** GUIs **MUST** have documentation explaining their use, as well as their design.

**Rationale:** Usage documentation allows users to learn how to use a GUI, and design documentation allows other developers to contribute to and maintain code.

## Documentation Links

**Description:** GUIs **MUST** have a component linking to documentation.

**Rationale:** This allows users to easily access documentation.

## Machine State Indication

**Description:** GUIs **SHOULD** indicate the state of the machine with which they are interaction, in particular which beam line.

**Rationale:** This makes clear what will happen when GUI actions are taken.

## Print To Log

**Description:** GUIs **SHOULD** have a print to log button that automatically saves its data.

**Rationale:** This ensures control or analysis using a GUI can be tracked by SLAC elogs, and that data is available to recreate figures.

## Data Save Location

**Description:** Data **SHOULD** be saved using the following format: ```$MATLABDATAFILES/YYYY/YYYY-MM/YYYY-MM-DD```, where ```$MATLABDATAFILES``` is an environment variable indicating the root directory for saving data. For example, on a production network, ```/u1/lcls/matlab/data/2023/2023-07/2023-07-18``` for files saved on July 18th, 2023. Consider using the util_dataSave function to take care of this.

**Rationale:** This is the default data directory for SLAC AD LCLS work.

## Tooltips

**Description:** GUIs **SHOULD** have tooltips that explain what components do.

**Rationale:** Tooltips make GUIs easier to use and understand and can serve as a sort of self-documentation.

## Containers

**Description:** GUIS **SHOULD** use containers (such as panels, group boxes and tabs) to group together components with a similar purpose. 

**Rationale:** Grouping related components together is more intuitive for the user and makes reorganizing the user interface easier for the developer as they can just move or resize the container as opposed to every individual component.

## Menus

**Description:** GUIs **SHOULD** have save, load and help menus at the top of a GUI in a menu bar drop down.

**Rationale:** This is standard practice in a large variety of applications and helps avoid cluttering the GUI with additional buttons.

## Visual/Style Guidelines

**Description:** These are recommended but optional practices:
- GUIs **MAY** have a dark motif, or a selectable dark mode.
- GUIs **SHOULD** have large enough font to be seen while standing behind an operator.
- GUIs **SHOULD NOT** have bright or flashing indicators.

**Rationale:** Dark themes typically provide more contrast, and larger font is easier to see from distance or with lower contrast. Bright or flashing elements distract from the GUI as a whole and generally don't provide extra value by virtue of being bright or flashy.

# Error Handling
All errors, both from MATLAB and in the functional execution of code, **MUST** be handled. Code **MUST NOT** exit in an uncontrolled manner; it should instead inform the user of the encountered issue in clear, easily understandable language. In the context of GUIs, error handling should be used in all callback functions to prevent the GUI from ever crashing. Error handling **SHOULD** communicate both what happened when a problem was encountered, and what the code was in the middle of doing when the error occurred. In general, one **SHOULD** display high-level error information in a dialog box and print relevant detailed information to a log. The recommended error handling practices are detailed below, and a code example of the error handling pattern can be found in the [Error Handling Example](./examples.md#Error-Handling).

## Callbacks 

**Description:** For GUIs, all callbacks **SHOULD** be wrapped in a try/catch loop.

**Rationale:** This helps prevent the GUI from crashing. Callbacks are used frequently and may encounter errors.

## Ignoring or Silencing Exceptions

**Description:** Code **SHOULD NOT** catch and ignore (or pass, or silence) errors; the user should always be informed of an error.

**Rationale:** Passing or ignoring exceptions makes it harder for a developer to maintain/debug code or for the user to understand when something is going wrong.

## Checking for Specific Errors

**Description:** In the catch section, code **SHOULD** check for specific types of errors and respond accordingly. These errors should have a message code string, in the MATLAB style, using a prefix id specific to your app. You use the id in the callback function to identify your app's exceptions. Code **SHOULD NOT** have one general catch statement that responds to all exceptions in exactly the same manner.
- If the exception is not one of the specific kinds your code looks for, the relevant information should be extracted from the stacktrace and communicated, including its location. As a developer, one should pay attention to what kinds of errors are common, and do their best to encapsulate them with custom error IDs.
- Use uiwait(errordlg(lprintf())) in the catch block to communicate the error information to the log and in a dialogue box.

**Rationale:** Code will often need to respond to different errors in different ways. Exceptions should also generally not cause stacktraces in the log (the ugly and largely useless "crash" stuff). Only the others, i.e. MATLAB’s own exceptions, should cause such stacktraces. In that way, errors like "division by 0" get a stacktrace, useful for debugging, whereas errors like "Couldn't move wire scan motor" do not, since that's not a bug in MATLAB, it's just something it wasn't able to do but handled gracefully.

## Using the Error Function

**Description:** In the algorithmic function, when an error is detected, code **SHOULD** immediately (on the next line after you detected the error) issue a message using the Matlab "error" function. The error() function's first argument should be the aforementioned message code string, and the second a more descriptive explanation. In every other (non-callback) method, especially API methods, use "throw(exception)". That is, whenever the program can't go on because of a functional problem, for example, you detect from EPICS that a wire is stuck, call error(). The method should also lprintf what happened.

**Rationale:** This function is built in to Matlab and automatically captures information about the error, storing information in an MException class data structure which can be thrown if desired. Using this function helps standardize error reporting and is already familiar to users of Matlab.

## EPICS Gets and Puts

**Description:** In general, code **SHOULD** check for error status on all EPICS gets and puts and implement appropriate error handling as described in the HLA Programmer's Guide [labca exceptions and error handling](https://www.slac.stanford.edu/grp/ad/docs/model/matlab/programmers_guide.html#labca) section.

**Rationale:** EPICS interaction can produce a variety of errors that need to be handled properly. Furthermore, when operating on devices in the field, it is critically important that the status/outcome of the operation is definitively known and clearly communicated to the end user. 


# Code Review and Release
There is a [code review and release procedure](https://www.slac.stanford.edu/grp/ad/docs/model/matlab/programmers_guide.html#appendix_b:_software_release_procedure) documented in the Matlab Physics Applications Programmers Guide. Please adhere to this procedure when releasing code to the production controls system. 

