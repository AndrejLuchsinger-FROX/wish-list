### Necessary information

**Active Application (checked out of Git Repo)**  
Table: “**sys_repo_config**”  
Field: “**sys_app**”  

**Active Branch Version with commit ID**  
Table: “**sys_repo_branch**”   
Fields:   
-	Name: “**name**”,   
-	ID: “**object_id**”,   
-	Active-mark: “**is_current**”  

**Uncommitted Changes per Branch**  
Table: “**sys_update_xml**”, “**sys_repo_stash**”  
Field: “**source_commit**”  
All updates per application are stored here. The Branch is not captured in the update record. when committed to the repo, it will be just committed to whichever Branch is checked out in studio.  
If the branch is changed before the changes are committed, studio will ask the user to store their changes in the local stash.   
Is an update recorded in a stash, the field “source_commit” stores the name of the stash in the form of its object_id.  

**Active Developer**  
Table: “**sys_update_xml**”, “**sys_repo_stash**”  
Field: “**ys_created_by**”  


### How changes are stored and used by the SN source control  
**Create an update:**  
In table “sys_update_xml_list” an entry for the new update is created. The application in the new record corresponds to the currently active application in studio. A new update set for the change will automatically be created. It is named after the user which created the update and stores all changes made by that user.

**Stashing an update:**  
If local changes are moved to the stash (if, for example, the branch is changed) two entries are created in the “sys_repo_stash” table.   
A parent record, which contains the customer updates (related list: Customer Updates in Batch) as well as a remote update set (related list: Retrieved Update Sets).  
And a second record, which again contain the customer updates (related lists: Customer Updates/Customer Updates in Batch).  
The update set which was created when first creating the changes, is now deleted again.  

**Loading updates from the stash:**  
The two entries in the “sys_repo_stash” table remain unchanged. A new record is created with state “Committed Stash”.  
An additional updateset is created, which contains all the updates stored in the stash which was just loaded. All other update sets which were connected to the stashed updates are deleted.  

**Committing the Updates:**  
The update set is closed and all the records in tables “sys_update_xml_list” and “sys_repo_stash” stay the same  
