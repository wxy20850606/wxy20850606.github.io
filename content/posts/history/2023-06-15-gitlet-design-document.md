---
title: "Gitlet design document"
date: "2023-06-15"
categories: 
  - "電腦科學"
keywords: 
- Gitlet
tags: # 标签
- Git
- 工具
- CS自學
weight:
slug: ""
draft: false # 是否为草稿
comments: true # 本页面是否显示评论
reward: true # 打赏
mermaid: true #是否开启mermaid
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏
showbreadcrumbs: true #顶部显示路径
cover:
    image: "" #图片路径例如：posts/tech/123/123.png
    zoom: # 图片大小，例如填写 50% 表示原图像的一半大小
    caption: "" #图片底部描述
    alt: ""
    relative: false
---

This is the design document for [Gitlet](https://sp21.datastructur.es/materials/proj/proj2/proj2) project.

# **Classes and Data Structures**

## **Main**

This is the entry point to Gitlet program. It takes in arguments from the command line and based on the command (the first element of the `args` array) calls the corresponding command in `GitletRepository` which will actually execute the logic of the command.

It also validates the arguments based on the command to ensure that enough arguments were passed in.

### **Fields**

This class has no fields and hence no associated state: it simply validates arguments and defers the execution to the `` `Gitlet`Repository`` class.

## **Blob**

GItlet, like Git, is a content-addressable filesystem. Even one byte change in content will lead to a totally different blob object.

To accomplish this, Blob class implements Serializable to save each different file content as blob object for future retrieve.

### **Fields**

All the following instance variable will be serialized.

- private String sha1; --> 40byte sha1 ID

- private File originalFile; --> original file path

- private File blobFile; --> new path to save each blob object in .gitlet/objects folder

- private String filename; --> filename for future use

- private String content; --> file content for future read or write into working directory

## **Commit**

**A commit is a snapshot in time**. To recover a corresponding working directory in the history, it use serialized commit object which contains a map to maintain all the filenames and their blob id.

The normal commit has one parent commits corresponding to the previous snapshots. A init commit has no parents and a commit with two parents is a merge commit.

### **Fields**

- private String timestamp; --> the date/time of the commit

- private String parent1; --> normal commit 's parent commit

- private String parent2; --> merge's commit 's second parent commit

- private String message; --> the message the commit

- private String commitID; --> the commitID of the commit

- private Map<String, String> fileToIDMap; -->a map with filename as key and commitID as value

## **Index**

Between two different working directories, there is a middle stage which records all the changes happened in between. Index class is for this purpose.

Different from Commit/Blob class, Index class only has one object saved in the .gitlet directory to maintain the changes in the staging area.

### **Fields**

The Index class records the added filenames and blobID map and the removed filename Set in staging area. It starts with empty map and treeset, record the added/removal information in the process and will be cleared after each commit.

- private final Map<String, String> added;

- private final TreeSet<String> removal;

## **Branch**

Branch class maintain all branch names and files information. It will use the branch name to create a file in .gitlet directory to maintain the head point of each branch.

### **Fields**

Branch class only have 1 instance varible “name".

- private String name

## **Utils**

This class contains helpful utility methods to read/write objects or `String` contents from/to files, as well as reporting errors when they occur.

This is a staff-provided and PNH written class.

### **Fields**

Only some private fields to aid in the magic.

## **GitletRepository**

This is where the main logic of our program will live. This file will handle all of the actual gitlet commands by reading/writing from/to the correct file, setting up persistence, and additional error checking.

It will also be responsible for setting up all persistence within gitlet. This includes creating the `.`gitlet folder as well as the folder and file where we store all commit/blob objects.

### **Fields**

Repo has following files and folders. More information in the following **Persistence** part.

public static final File _CWD_ \= new File(System._getProperty_("user.dir"));  
public static final File _GITLET\_FOLDER_ \= _join_(_CWD_, ".gitlet");  
public static final File _HEAD\_FILE_ \= _join_(_GITLET\_FOLDER_, "HEAD");  
public static final File _INDEX\_FILE_ \= _join_(_GITLET\_FOLDER_, "index");  
public static final File _LOG\_FOLDER_ \= _join_(_GITLET\_FOLDER_, "logs");  
public static final File _LOG\_HEAD\_FILE_ \= _join_(_LOG\_FOLDER_, "HEAD");  
public static final File _OBJECT\_FOLDER_ \= _join_(_GITLET\_FOLDER_, "objects");  
public static final File _REFS\_FOLDER_ \= _join_(_GITLET\_FOLDER_, "refs");  
public static final File _REFS\_HEADS\_FOLDER_ \= _join_(_REFS\_FOLDER_, "heads");  
public static final File _REFS\_HEAD\_MASTER\_FILE_ \= _join_(_REFS\_HEADS\_FOLDER_, "master");

# **Algorithms**

### **Main**

Main class contains the most important entrance method for whole project. Main method use "switch" to handle different cases.

- public class Main {}

It also has four private methods to help validate the arguments.

- private static void validateNumArgs(String\[\] args, int n);

- private static void validateMessage(String\[\] args, int n)

- private static void validateIfInitialized()

- private static void validateNotInited()

### **Blob**

Blob class have three public methods for other class to use and one private method .

- public String getContent() --> Get content from blob object.

- public String getSHA1() --> Get 40 bytes BlobID from blob object.

- public void save() --> Save blob object to make it permanent.

- private File getBlobFile() --> This is a helper method to aid save() method.

### **Commit**

All the public methods :

- public String getMessage() --> get commit message

- public String getTimestamp(). --> get commit timestamp

- public Commit getParent1() --> get commit 's parent commit

- public List<Commit> getParent() --> get commit 's parent commit list

- public Commit getParent2() --> get commit 's parent2 commit

- public Boolean havaParent1() --> check if have parent1

- public Boolean havaParent2() --> check if have parent2

- public String getParent2ID() --> get commit parent2 ID

- public String getParent1ID() --> get commit parent1 ID

- public Map<String, String> getMap() --> get commit map

- public void makeCommit(). -->create the commit

- public void save() --> save the commit object

All the following public static methods are for other class to use.

- public static Map<String, String> getLastCommitMap()

- public static Commit getLastCommit()

- public static File getHeadPointerFile()

- public static String getHeadPointer()

All the private static method are used to support above methods.

- private static void writeToGlobalLog(Commit x)

- private static String formatDate()

- private static void updateHeadPointerFile(String commitID)

### **Branch**

Branch class only have one method to create a branch.

public void create()

## **Index**

- public void add(String filename, String sha1)

- public void removeFromRemoval(String filename)

- public void remove(String filename)

- public void stageRemoval(String filename)

- public void save()

- public Map<String, String> getMap()

- public TreeSet<String> getRemoval()

- public void clear()

- public boolean stagingAreaFlag()

- public static Index readStagingArea()

- public static Map<String, String> readStageMap()

- public static TreeSet<String> getStageRemoval()

### **GitletRepository**

**GitletRepository** is where the main logic lives.

The following are the steps and inner steps of each method.

1.init

public static void init() 

steps：

- make directory --> use method: _private static void mkDir()_

- Initialize all the needed objects --> use helper method: private static void initializeNeededObject()

Helper method initializeNeededObject() will do the following:

- _make initial commit_

- _initialize index object and serialize it._

- _write .gitlet/HEAD file_

- _write .gitlet/refs/heads/master file_

- _write .gitlet/logs/HEAD file_

edge cases:

already handled in the main class.

2.add

public static void add(String filename) 

steps:

- check if filename is existed or not --> use method: private static boolean checkFileExistence(String filename)

- process if filename exist

- exist if not exist

There is a helper method handleIfFileExist check all the conditions and handle them accordingly.

private static void handleIfFileExist(String filename) 

- check if file is _identical to the version in the current commit_

- _check if file is in removalSet_ in Index object

- _save the correct blob and add it to staging added map_

3.commit

steps:

- _If no files have been staged, abort._

- calculate the corresponding commit map

- create commit object and save it

- clear the staging area

There is a helper method to calculating the new commit map according to following steps:

- combine _last commit map add stagingArea map_

- _minus rm file_s

4.rm

public static void rm(String filename)

steps:

- check if the given file is staged for added or not and make changes if needed

- check if the given file is staged for removal or not and make changes if needed

- check if the given file is _tracked by the head commit_ and make changes if needed

- save the index file to update the staging area

5.log

public static void log() 

steps:

- loop through the commit map until reach the init commit to print out all log information.

-  use private static void printCommitLog(Commit x)

6.global-log

public static void globalLog() 

steps:

No need to process ,just print out the global log file.

7.find

public static void find(String message)

steps:

- get all the files list in .gitlet/objects folder --> use private static List<String> getFileNameList(File dir)

- Because both blob object and commit object are saved in .gitlet/objects folder, use "try, finally" to filter out the commit object and then process.

8.status

public static void status() 

steps:

- use statusBuilder to keep all the status contents

- read from index object and working directory to get needed information

- Use Collections._sort_ to alphabet the list

- print the status

8.checkout

  1. checkout -- \[file name\]

public static void checkoutFilename(String filename)

Use helper method _writeFileByCommit_(_getLastCommit_().getCommitID(), filename);

Helper method steps:

- _overwrite the file if it exists in the working directory_

- _create the file if it is not in the working directory_

- exit if _filename not exist in current commit_

  2. checkout \[branch name\]

public static void checkoutBranch(String branchName) 

steps:

- _exit if have untracked_ files

- _handle other checkout failure cases_

- remove and delete files accordingly

- _update the HEAD file_

There is a helper method :private static void recoverAndDeleteFiles(String branchName).

- _recover all the files_

- delete if exist in current branch but not in the given branch

  3. checkout \[commit id\] -- \[file name\]

public static void checkoutCommit(String commitID, String filename)

- _update the uid if user input short uid_

- _exist if not exist_

- _if exist, write file_, use helper method : _writeFileByCommit_

9. branch

public static void branch(String branchName)

- check if branch exist

- create branch

10. rm-branch

public static void rmBranch(String branchName)

- _If a branch with the given name does not exist, aborts_

- _If you try to remove the branch you’re currently on, aborts._

- _Delete the branch with the given name._

11. reset

public static void reset(String commitID) 

- _If a working file is untracked in the current branch_

- _handel short uid_

- _clear cwd_

- _recover cwd_

- _clear staging area_

12. merge

public static void merge(String branchName) 

steps:

  1. Check all failure cases.

Use helper method: _handleFailureCases_(branchName).

  2. Find the split commit, use helper method: _getSplitPointID_(currentHead, targetHead)**

- Create a helper function: _getCommitDepthMap_

- Calculate each commit's depth, add commit ID as key and depth as value into _CommitDepthMap_.

- Compare two depth maps to find the split commit

  3. Check If the split point is the target branch head**

Use helper method：

private static void ifSplitIsGivenBranch(Commit split, Commit target)

  4. Check if _the split point is the current branch head_**

Use helper method：

private static void ifSplitIsCurrentBranch(String branchName, Commit split, Commit current) 

  5. Combine three maps to get all filename list.**

Use helper method：

private static Set<String> getfileNameSet(Commit a, Commit b, Commit c)

  6. Calculate the new filename/blobID map according to 8 rules.**

private static Map<String, String> getNewMergeMap(Commit cur, Commit tar, Commit spl)

Steps:

     1. identify conflict cases

private static boolean haveConflict(String fileName, Commit cur, Commit tar, Commit spl) 

There is a conflict in the following four conditions:

- in split commit but modified in target commit and remove in current commit

- _in split commit but modified in current commit and remove in target commit_

- _not in split, the contents of both are modified and different from the other_

- _in all three commit, the contents are different from each other_

    2. handelMergeConflict cases

- calculate the conflict contents by combine two blobs

- write the content to the filename in the working directory

- create new Blob object

- save the Blob object in the .gitlet/objects folder

private static String handelMergeConflict(String filename, Commit cur, Commit tar)

    3. handle other cases which contain changes

- _in all, modified in current, not modified in target_

- _in all, modified in target, not modified in current_

- _not in split, not in current, but in target branch_

- _not in split, not in target, but in current branch_

    4. overlook the rest if it is no need to put in new map

7. compare the new map with current branch head commit map to handle the difference**

- stage for remove ,delete file

- stage for add ,write file to working directory

- overlook the rest cases because there is no need to take action.

8. _make merge commit_**

9. _clear staging area_**

## **Persistence**

The directory structure looks like this:

```
CWD                             <==== Whatever the current working directory is.
└── .gitlet                     <==== All persistant data is stored within here
    ├── HEAD                    <==== "refs/heads/branch name",to get current branch 's head commitID in future
    └── index                   <==== All staging area's information are stored in this directory
    └── logs 
        ├── HEAD                <====A file to store all the commit history  
    └── objects                 <==== A single Blob/Commit instance stored to a file
        ├── First 2 numbers of BlobID 1 or CommitID 1 as folder name 
            ├── Last 38 numbers as file name
        ├── First 2 numbers of BlobID 2 or CommitID 2 as folder name 
            ├── Last 38 numbers as file name
        ├── First 2 numbers of BlobID 3 or CommitID 3 as folder name 
            ├── Last 38 numbers as file name
        └── ...
            ├──...
    └── refs
        └── heads
           └── master           <====A file to store the last commitID of master branch
           └── other branch 1   <====A file to store the last commitID of other branch
           └── other branch 2   <====A file to store the last commitID of other branch
           └── ...
```
