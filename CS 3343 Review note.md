# CS 3343 Review note

## Week 02  
**Test Automation & Making Software Testable**

1. Learn the **Test Procedure**
2. **Understand** the structure of software to make your application classes testable
3. **Apply** Test Automation


- Testing can only **show** the presence of errors
- It **can not** prove the absence of all errors

### Procedure for Software Testing
1. Defining Test Objectives
2. Test Design -> Define the requirements to be test
3. Test Execution -> Junit
4. Test Analysis -> Measure how well the test execution achieves

- For creating isolated tests -> using test stubs

- 关于stub mock fake dummy的区别见evernote， unittest tag

- Testing a GUI-based JAVA Application 

## Week 03 
**Project Management**

Software Project Management in Practice :
- Iterative; progressively evolving
- Apply Good Software Patterns
- Avoid Anti-patterns (Knows to cause problems)

Mini Trade-off Triangle:
- Fast, Cheap and Good. You get to Choose two only.
- Product, Schedule and Cost


### What can be considered as a Successful project?
- Completed within allocated **time** frame.
- Completed within allocated **budget** 
- Customer Requirements **satisfied** exceeded
- Accepted by the customer

Failed Project:
- Over-time
- Over-budget
- Poor quality
- Rejected by the customer


### Key Reasons of Our Projects Failures
- Ever-changing project scope(**keep changing**)
- Poor requirements gathering(**incorrect requirements**)
- Guess-a-schedule rather than plan-a-schedule(**no plan**)
- Lack of management support(**supports and impacts**)
- Unrealistic planning and scheduling (**Forecasting/planning failure**)
- Lack of resources(**no money, no people, no tools**)
- Lack of required technical skills(**Inadequate experience**)
- Ineffective team members(**Ineffective team composition/task allocation**)

Corresponding Solutions in Project
- Fix and decide on your project in Week3 (误
- Focus on the core functionality
- Split tasks until one member can complete the task in 1-2 weeks.(decomposition)
- Seek helps from teachers
- Report real progress and compare to project plan.
- Reduce unrealistic tasks.
- Reduce the difficulty of the project that is workable
- Study new technologies together during the project
- Better team building, and effective task allocation according to skills.


**Gantt Chart** （一种管理任务的图表）

### 4 Project Dimensions to consider
- People
    - Developer **productivity** may be varied
    - Assign the right people to the right task
- Development **Process**
    - Follow **a software process** so that everyone knows what is going on
    - Avoid skipping important tasks, such as testing
    - Task-oriented development
- **Product** Focus
    - Manage **the code and document** with respect to the requirements
- **Technology**
    - Use **right tools and design patterns** to gain productivity

#### How to handle these 4 dimensions?
- Select Proper Development Model
    - We use a **test-driven approach** in our project.
    - Every function developed must be tested
    - **Schedule-Oriented** Practices
- Manage Risks
    - Avoid Classic Mistakes
    - Follow software development fundamentals
- Software design
    - Follow design principles and patterns

#### Schedule-Oriented Practices
- Understand the constraints: Cost, Effort, Schedule
- Planned vs. Actual 
- The absolute residual/error (现实与预测间的误差）

#### Avoid the 36 Classic Mistakes
- [McConnell's Anti-Patterns](http://www.stevemcconnell.com/rdenum.htm) _有时间再读吧-. -_
- Types:
    - People-Related
    - Process-Related
    - Product-Related
    - Technology-Related

- People-Related Mistakes
    - Damaging the team's motivation or morale
    - Weak Personnel
    - Uncontrolled problem employees
    - Heroics
    - Adding people to a late project
    - Noisy, crowded offices
    - Customer-Developer friction (🌚???
    - Unrealistic Expectations
    - Wishful thinking
- Process-Related Mistakes
    - Optimistic schedules
    - Insufficient **risk management**
    - **Contractor** failure
    - Insufficient planning
    - Abandonment of a plan under pressure
    - Wasted time during fuzzy front end
    - Shortchanged(缩手缩脚) analysis and design activities
    - Inadequate design considerations
    - Shortchanged quality assurance
    - Insufficient management controls
    - Frequent convergence of intermediates and final deliverables
    - Omitting necessary tasks from project estimates
    - Planning to catch-up laster
    - **Code-like-hell** programming (free style...
- Product-Related Mistakes
    - Requirements gold-plating
    - Feature creep
    - Beware of the pet project
    - Push-me, pull-me negotiation
    - Use the project to test risky dieas
- Technology-Related Mistakes
    - Silver-bullet syndrome -> There is no silver bullet !!!!!!!!!
    - Over-estimated savings from new tools and methods
    - Switching tools in mid-project, strategically incorrect.
    - Lack of automated source-code control, subversion/github

    
#### Organizational Structures in a company
- Functional 
    - Engineering, Marketing, Design, etc
    - Profit & Loss from production
- Project-based
    - Project A, Project B ...
    - PM has Profit & Loss responsibility
-  Matrix
    -  Functional and Project based
    -  Program management Model
    -  Shorter cycles, needed for rapid development process

- Functional Organization
    - Pros
        - Clear definition of authority
        - Eliminated duplication 
        - Encourages specialization
        - Clear career paths
    - Cons
        - "Walls": can lack customer orientation
        - "hierarchy" creates longer decisions cycles
        - Conflicts across functional areas
        - Project leaders have little power
- Project-based Organization
    - Pros
        - Unity of Command
        - Effective inter-project communication
    - Cons
        - Duplication of facilities
        - Career path
- Matrix Organization
    - Pros
        - Project integration across functional lines
        - Efficient use of reousrces
        - Retains functional teams
    - Cons
        - Tow bosses for personnel
        - Complexity
        - Resource & priority conflicts

## Week 04
**Project Planning and Scheduling**
### Facts of Project Planning and Scheduling
- Mythical Man-Month Principle
- "Cost varies, as product of men and months, progress does not"
- "Adding manpower to a late project makes it later..."

### How to Schedule?
1. Identify "what" needs to be done
    - Work Breakdown Structure (WBS)
2. Identify "how much" (software size)
    - Software size estimation techniques
    - Such as lines of code, functions, requirements
3. Identify the **dependency** between tasks
    - Dependency graph, network diagram
    - Minimize task dependencies to avoid delays
4. Estimate total duration of the work to be done
    - The actual schedule
    - Task allocation
### Work Breakdown Structure: WBS
- Hierarchical list of project's work activities
- 2 Formats
    - Outline (indented format)
    - Graphical Tree(Organizational Chart)
- Uses a decimal numbering system
    - 0 is typically top level
- Includes
    - Development, mgmt., and project support tasks
- **First 3 levels should be sufficient** for our course project. 

#### Techniques to Produce WBS
4 ways: 
    - Top-Down (Big Picture First)
    - Bottom-Up (Details First)
    - Analogy (Similarity, By referencing)
    - Post-its on a wall (brainstorming)

    
|  | Top-down | Bottom-up | Analogy | Brainstorming|
| --- | --- | --- | --- | --- |
| Starting Point  | Start at highest level | Start at lowest level | Collect similar project | Brainstorm all activities you can think of that need to be done|
| What to do ? | Fill in details | Consolidate into summaries and higher levels | Modify the "template" projects | Group them into categories|

Bottom-Level Tasks (of WBS)
    - Generic term for discrete tasks with definable end results
    - Typically the "leaves" on the tree
    - **The "one-to-two" rule**
        - **Often at : 1 or 2 persons for 1 or 2 weeks
    - Basis for monitoring and reporting progress
        - Can be tied to budget items (charge numbers)
        - Resources (personnel) assigned
    - Ideally shorter rather than longer
        - longer makes in-progress estimates needed
        - These are more subjective than "done"
        - 2-3 weeks minimum (occasionally a half day)
        - Not so small as to micro-manage

Bar charts(Project **Gantt Char**) and activity networks
    - Illustrate project schedule
    - Show project breakdown into tasks
    - Activity charts show task dependencies and critical paths.
    - Bar charts show schedule against calendar time. 

Project Scheduling
    - Once tasks (from the WBS) and size/effort (from estimation) are known: then schedule
    - Primary objectives
        - Best time
        - Least Cost
        - Least Risk
    - Secondary objectives
        - Evaluation of schedule alternatives
        - Effective use of resources
        - Communications

#####   Critical Path Method (CPM)
* "The specific set of sequential tasks which the project completion date depends"
    * or "The longest full path"
* All Projects have a Critical Path
* accelerating non-critical tasks do not directly shorten the schedule

## Week 07
**Code Refacting**

### Code Smell: Duplicated Code
- Extract method
- Pull Up Field
- Form Template Method
- Substitute Algorithm 
- Extract class


### Code Smell: Long Method 

- Extract method
- Replace Temp with Query
- Introduce Parameter Object 
- Replace Method with Method Object
- Decompose Conditionals

### Refactoring 
- Write a set of test cases!
- Looking for code smell 
- If any case failed, debug the program or fallback to your original code.
- Smell the codes and do refactoring until we do not find serious symptoms. 

### Code Refactoring Template 
- Encapsulate Field
- Introduce NULL Object 
- Parameterize Method
- Extract Subclass 
- Extract Interface
- Pull Up Method 
- Replace Type Code with State/Strategy 
- Replace Inheritance with Delegation
- Hide Deletgate

## Week 08
**Test Comprehensiveness**    
- Exhaustive Testing: Test all possible inputs.    
- Random Testing: Choose test cases Randomly among all possible inputs.   


### Code Coverage 
Code coverage is the **percentage** of a particular kind of program entities.   

### Code-Based Control Flow Testing 

#### Control-flow   
- Statement Coverage 
- Branch Coverage 
- Loop Coverage 
- Path Coverage 

#### Predicate 
- Condition Coverage (CC)
- Decision Coverage (DC)
- C/DC
- MC/DC




