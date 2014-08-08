---
layout: default
category: jingles
section: jingles/usermanagement
title: Jingles Usermanagement
---
# Jingles User Management

## Users<a id="users"></a>

### list view<a id="user-list"></a>

![](/assets/img/jingles/users01.png)

The default list view shows you a list of users that already exist in the system:

- **Name** The users name.
- **Roles**  The roles the user belongs to.
- **Orgs**  The organisations the user belongs to, the active one ins in **bold**

Clicking on the hyperlinked users name will take you to the users detail page as explained in a subsection below.

### creation<a id="user-new"></a>

To create a user click on the ![add user](/assets/img/jingles/users-add.png) button.

This will take you to the new user creation page as shown below.

![](/assets/img/jingles/users02.png)

Enter a username for your new user and please note that usernames are case sensitive. Enter a password for your new user by typing in the password twice. Only if the 2 passwords are identical will the "create" button appear. To complete user creation click on the create button.

### details<a id="user-details"></a>

Clicking on an individual hyperlinked user name will take you to the user details page.

![](/assets/img/jingles/users03.png)

1. The name of the user you are working on.
2. Tab sectioned relating to the user. Explored in more detail further below.
3. The user details tab shows you the uuid of the user as well as allows you to change the users password.
4. Clicking on the trash icon ![delete user](/assets/img/jingles/users-delete.png) will launch a popup dialogue that will ask for you to confirm user deletion.

#### Permissions

![](/assets/img/jingles/users04.png)

FiFo includes a very powerful fine grained permission system that lets you tailor specific permissions for specific users or roles. This allows you to control what areas within the "User Interface" users can see and what actions they may perform on different objects such as machines, datasets, packages etc.

It is beyond the scope of this section to explain the permission system in necessary depth. For further information please consult the permissions wiki article.

#### Roles

![](/assets/img/jingles/users05.png)

The roles section allows you to add users to certain pre-existing roles. Users who get assigned to a specific role will inherit all the permissions and access rights of that role.

#### SSH Keys

![](/assets/img/jingles/users06.png)

<p class="bs-callout bs-callout-danger">
Note that SSH keys need to have the form 'ssh-rsa key-data key-id', other key, while valid will be refused.
</p>

You can add your own ssh public key to your user profile by simply pasting in your key and clicking the save button. This will be used when the user creates a new machine and FiFo will automatically add the users key to the machines authorized_keys file. You should then be able to authenticate your ssh session without requiring password entry.

## Roles<a id="roles"></a>

### list view<a id="role-list"></a>

![](/assets/img/jingles/roles01.png)

The "Roles" section is very similar to the "users" section and is the place within FiFo where you create roles and manage role permissions.

To edit an existing role simply click on the roles name hyperlink. This will take you to the role details section.

### creation<a id="role-new"></a>

To add a new role click on the "add role" button.

![](/assets/img/jingles/roles02.png)

Enter a name for your new role and click on the "create" button.

### details<a id="role-details"></a>

To edit a role click on the role name hyperlink in the role list view.

![](/assets/img/jingles/roles03.png)

1. The name of the role you are working with.
2. The Tab's available for the role.
3. The Details view shows the unique UUID of the role.

![](/assets/img/jingles/roles04.png)

FiFo includes a very powerful fine grained permission system that lets you tailor specific permissions for specific users or roles. This allows you to control what areas within the "User Interface" users can see and what actions they may perform on different objects such as machines, datasets, packages etc.

It is beyond the scope of this section to explain the permission system in necessary depth. For further information please consult the permissions wiki article.

## Orgs<a id="orgs"></a>

### list view<a id="orgs-list"></a>


### creation<a id="orgs-create"></a>

### details<a id="orgs-details"></a>
