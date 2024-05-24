# Setup WordPress With Security Best Practice
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2Fpassword123456%2Fsetup-wordpress-with-security-best-practice&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)

This document is written with the purpose of being suitable for web applications developed using WordPress, where there is no interaction with users. 
Typically, it applies to corporate brand pages, various static views, recruitment pages, and the like.

For websites where users register (sign up) and freely use the site, such as open communities, some items in this document may not be applicable. Please keep this in mind as you read.

It does not encompass all the content necessary for securing WordPress. However, it is written to a level where it includes general but detailed information, allowing for security risk assessments and vulnerability responses based on the guide.
***

==== In Progress as of May 23, 2024. ====

If you find this helpful, please the **"star"**:star2: to support further improvements.

## 1. Ensure that the Default WordPress Admin Username Has Been Changed

When you install WordPress, the default admin username is "admin" unless you change it during the setup process. 
The "admin" account name is widely known, so it should be changed to a different name.
If you continue to use "admin" as your admin username, an attacker could attempt a brute-force attack using "admin" to gain access to your WordPress site.

If an attacker gains access to the WordPress admin account, they will have full control over the website.
The default WordPress admin username should be changed to a different name.

**Audit:**
- Verify that if the default WordPress admin username is still set to "admin".

**Remediation:**
- If username is "admin", change it to a less predictable username immediately.

1. Login into your WordPress admin dashboard using admin account.
2. "Users" area from your dashboard panel, and click on "Add New User".
3. Fill in the form and choose "administrator" in the "Role" drop down menu (remember to use a strong web password and also use the provided password strength indicator to confirm 
that your new password is strong enough).
4. When finished, click on the "Add New User" button.
5. Log in again using your new WordPress admin username.
6. Navigate to the "Users" area again.
7. In the users list select the previous “admin” username and select "Delete" from the drop-down menu.
8. When deleting the old admin, you will be asked about the articles posted under the previous "admin" username. 
   - Select the option "attribute all posts and links to:" and select your new administrator. 
   - When all set, click "Confirm Deletion".

**Note:**
Always use different "display name" from the username. If the actual username is used as display name of the content author, a hacker will easily identify username and target the account


## 2. Ensure User Roles and Permissions in WordPress are Properly Managed
By default, WordPress has five user roles - "Administrators", "Editors", "Authors", "Contributors", "Subscribers"

These roles allow you to control what tasks users can perform on your website by assigning appropriate permissions. 
If user roles and permissions are not properly managed, users might gain unnecessary access to critical functionalities, posing a significant security risk.

**Audit:**
- Verify that WordPress user roles and permissions are adjusted to fit the needs of your website.
- Review all user roles to ensure they are aligned with the current operational policies of your website.

**Remediation:**
- Assign and manage user roles according to the needs of your website.
- Generally, WordPress should be operated with one Administrator, one Editor, one Author.
- Remove unnecessary admin accounts or reduce permissions where needed.
- Regularly review users and their roles to ensure they are up-to-date with any changes.

| no | Role           | Description                                                                                    | Administrative Staff                                        |
|----|----------------|------------------------------------------------------------------------------------------------|-------------------------------------------------------------|
| 1  | Administrator  | Has full access to all WordPress features and can manage all content on the site.              | System Admins                                               |
| 2  | Editor         | Can manage and publish other users' posts, as well as edit and publish content.                | Operational Service Managers, Internal Content Contributors |
| 3  | Author         | Can write and publish their own posts, and has permission to edit their own posts.             | Internal Content Contributors                               |
| 4  | Contributor    | Can write content but cannot publish it. Posts are reviewed and published by an administrator. | Internal Content Contributors                               |
| 5  | Subscriber     | Can log in to the site and manage their personal profile, but cannot write or edit content.    | Internal Content Contributors                               |

**Note:** For service-oriented websites such as company blogs, recruitment pages, brand sites, and promotional sites where users interact minimally and content is primarily showcased, roles such as "Administrators", "Editors", and "Authors" are typically sufficient.



## 3. Ensure User Registration is Disabled
WordPress includes a built-in user registration feature. This feature is disabled by default, but it can be activated by an administrator.

If this feature is enabled, anyone can register and potentially access the WordPress Admin dashboard, which can lead to security issues.
For most websites that are not intended to operate as open communities, the user registration feature is unnecessary and should remain disabled.

**Audit:**
- Verify that user registration is disabled. You can check this by attempting to access the user registration page.

1. Using a web browser
   - Go to https://yourwordpress.com/wp-login.php?action=register
   - If user registration is disabled, you will see "User registration is currently not allowed."
    ![3.1!](/images/3.1.png)

2. Using curl
   - If user registration is disabled, it will redirect to the disabled page.

```
# curl -i -k "https://yourwordpress.com/wp-login.php?action=register"

(response)
HTTP/1.1 302 Moved Temporarily
cache-control: no-cache, no-store, must-revalidate, max-age=0
content-type: text/html; charset=UTF-8
server: Apache
content-length: 0
...
location: https://yourwordpress.com/wp-login.php?registration=disabled
```

**Remediation:**
- If user registration is enabled, disable it.
- Uncheck “Anyone can register”
![3.2!](/images/3.2.png)


## 4. Ensure File Editor is Disabled
If an attacker breaks into a WordPress Administrator account, they can take full control of your website. 
They can edit the coding of your theme and plugins through the built-in "Editor" feature, upload malicious scripts, deface your site, spam your users, and more.

Common hacks via these editors include SQL injections, SEO spam hacks, and Japanese SEO spam.

**Audit:**
- Verify that the file editor is disabled. 
- Check if you can access the editor via Appearance > Editor or Plugins > Plugin Editor.

**Remediation:**
- If the file editor is enabled, disable it by following these steps:

1. Access your wp-config.php file using File Manager or FTP
2. Open the wp-config.php file for editing.
3. Scroll down to the bottom of the file (if using the default wp-config.php).
4. Locate the following line:
`
/* That’s all, stop editing! Happy publishing. */
`
5. Above this line, add the following code:
`
define('DISALLOW_FILE_EDIT', true);
`
6. Save the changes and close the editor.
7. Return to your WordPress dashboard and confirm that the editor options are no longer available.

**Note:** 
If you do not have access to cPanel, you can download your wp-config.php file via FTP, edit it in a text editor to include the above line of code, and then upload it back to your website, overwriting the old file.

Alternatively, use a security plugin like MalCare that includes a "Disable File Editor" feature to simplify this process.

