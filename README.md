# Activity 11: Deploy Folder Redirection Using GPO

Redirecting the Documents folder to a network share via Group Policy, so a user's files follow them to any domain-joined computer.

## Background
Folder redirection points a known folder (e.g. Documents) at a network location instead of the local drive. Users interact with it exactly as if it were local, but the files are actually stored centrally and available from any computer on the network.

## Steps

### Set up the shared folder
- [ ] On the domain controller, create a folder `C:\FOLDERDIR` (no spaces in the name)
- [ ] Server Manager → Files and Storage Services → Shares → New Share → SMB Share – Quick
- [ ] Select the server and the `FOLDERDIR` path
- [ ] Append `$` to the share name (`FOLDERDIR$`) to make it a hidden share
- [ ] Copy the resulting network path
- [ ] Enable access-based enumeration
- [ ] Customize permissions → Disable inheritance → Convert to explicit permissions
- [ ] Remove existing permission entries
- [ ] Add principal: Mike Smith
- [ ] Under advanced permissions, check **Create folder/append data** and uncheck **Traverse folder/execute file**
- [ ] Apply to this container and its objects/containers
- [ ] Finish and create the share
- [ ] Verify in Server Manager → Shares that `FOLDERDIR` was created

### Create the GPO
- [ ] Tools → Group Policy Management
- [ ] Right-click Group Policy Objects → New → name it `Folder Redirection GPO`
- [ ] Edit the GPO
- [ ] Expand User Configuration → Policies → Windows Settings → Folder Redirection
- [ ] Right-click Documents → Properties
- [ ] Change setting from "Not configured" to **Basic**
- [ ] Paste the copied network path under Root Path
- [ ] Under Settings, choose "Redirect the folder back to local user profile" for policy removal (note: even the administrator won't have access to the user's folder by default)
- [ ] Apply and close the editor
- [ ] Link the GPO to the `XYZ Users` OU

### Test
- [ ] Log into the client PC as `msmith`
- [ ] Run `gpupdate /force`
- [ ] Log back in as `msmith`
- [ ] Create a new folder inside Documents, then check the folder's location bar to confirm the redirected path
- [ ] Confirm the Desktop/Documents icons show green (online) status
- [ ] On the server, browse to `C:\FOLDERREDIR\msmith` to confirm the data lives there
- [ ] Confirm that accessing another user's redirected Desktop folder is denied, even as administrator

## Screenshots to capture
- [ ] Share permissions setup
- [ ] Folder Redirection GPO settings
- [ ] Confirmation of redirected folder path on the client

<img width="973" height="1094" alt="image" src="https://github.com/user-attachments/assets/425a1b23-5871-46de-954e-d0d96ecb4783" />

<img width="973" height="1095" alt="image" src="https://github.com/user-attachments/assets/7a416993-4e3f-420b-98f5-733b5f62181d" />

<img width="973" height="1095" alt="image" src="https://github.com/user-attachments/assets/e4b0ad09-c33d-4ce6-bc15-9d5970c8cbc6" />


