.. Project-FiFo documentation master file, created by
   Heinz N. Gies on Fri Aug 15 03:25:49 2014.

***************
User Management
***************

To get to *Fifo's* user management section: 

1. Click **More** on the home screen and then select **Configuration**.

2. On the next page select **Users and Roles**

.. image:: /_static/images/jingles/users_getto.png

Users
#####

.. todo::

	YubiKeys - Users & Organizations - Users subsection needs to be added.

List View - Users
*****************

.. image:: /_static/images/jingles/users01.png

The default list view shows you a list of users that already exist in the system:

* **Name** The users name.
* **Roles**  The roles the user belongs to.
* **Orgs**  The organisations the user belongs to, the active one ins in **bold**

Clicking on the hyperlinked user's name will take you to that user's detail page as explained in a subsection below.

Creation - Users
****************

To create a user click on the |new user| button.

.. |new user| image:: /_static/images/jingles/users-new.png

This will take you to the new user creation page as shown below.

.. image:: /_static/images/jingles/users02.png

Enter a username for your new user. Please note that usernames are case sensitive. Enter a password for your new user by typing in the password twice. Only if the two passwords are identical the |create button| button will appear. To complete user creation click on the |create button| button.

.. |create button| image:: /_static/images/jingles/create.png

Details - Users
***************

Clicking on an individual hyperlinked user name will take you to that user's details page.

.. image:: /_static/images/jingles/users03.png

1. The name of the users you are working on.
2. Tab sectioned relating to the user. (Explained in further detail below).
3. The user details tab shows you the uuid of the user, allows you to change that user's password, and select the active organisation they are working for.
4. Clicking on the |trash icon| will launch a popup dialogue that will ask for you to confirm user deletion.

.. todo::

   Split out 3. into 3 sections:

   1. Information (name/uuid)
   2. Active organisation
   3. Password fields & change  button

.. |trash icon| image:: /_static/images/jingles/users-delete.png

Permissions - Users
*******************

.. image:: /_static/images/jingles/users04.png

*FiFo* includes a very powerful fine grained permission system that lets you tailor specific permissions for specific users or roles. This allows you to control what areas within the "User Interface" users can see and what actions they are permitted to perform on different objects such as machines, datasets, packages etc.

It is beyond the scope of this section to explain the permission system in necessary depth. For further information please consult the `Rights Management <../general/rightmanagement.html>`_ section.


Roles - Users
*************

.. image:: /_static/images/jingles/users05.png

The roles section allows you to add users to certain pre-existing roles. Users who get assigned to a specific role will inherit all the permissions and access rights of that role.

SSH Keys - Users
****************

.. image:: /_static/images/jingles/users06.png

.. attention::

	SSH keys need to have the form 'ssh-rsa ``key-data`` ``key-id``', keys with another format - while valid - will be refused.

You can add your own ssh public key to your user profile by simply pasting in your key and clicking the save button. This will be used when the user creates a new machine and *FiFo* will automatically add the users key to the machines authorized_keys file. You should than be able to authenticate your ssh session without requiring password entry.

____

Roles
#####

List View - Roles
*****************

.. image:: /_static/images/jingles/roles01.png

The **Roles** section is very similar to the **Users** section and is the place within *FiFo* where you create roles and manage role permissions.

To edit an existing role simply click on the roles name hyperlink. This will take you to the role details section.

Creation - Roles
****************

To add a new role click on the |new roles| button.

.. |new roles| image:: /_static/images/jingles/roles_new.png

.. image:: /_static/images/jingles/roles02.png

Enter a name for your new role and click on the |create button2| button.

.. |create button2| image:: /_static/images/jingles/create.png

Details - Roles
***************

To edit a role click on the role name hyperlink in the role list view.

.. image:: /_static/images/jingles/roles03.png

1. The name of the role you are working with.
2. The Tab's available for the role.
3. The Details view shows the unique UUID of the role.

.. image:: /_static/images/jingles/roles04.png

*FiFo* includes a very powerful fine grained permission system that lets you tailor specific permissions for specific users or roles. This allows you to control what areas within the **User Interface** users can see and what actions they may perform on different objects such as machines, datasets, packages etc.

It is beyond the scope of this section to explain the permission system in necessary depth. `Permissions <https://project-fifo.net/display/PF/Permissions>`_ wiki article.

.. note::
	The `Permissions <https://project-fifo.net/display/PF/Permissions>`_ wiki article is not filled with content yet.

____

Orgs
####

.. attention::

	content is missing

List View - Orgs
****************

.. attention::

	content is missing

Creation - Orgs
***************

.. attention::

	content is missing

Details - Orgs
**************

.. attention::

	content is missing
