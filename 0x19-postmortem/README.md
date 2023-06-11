POSTMORTEM
In the release of the ALX Systems Engineering & DevOps Project 0x19, around 06:00 Nigerian Time, I encountered an inaccessible Ubuntu 14.04 container running an Apache Web Server. In the process of trying to Get Requests from the server led to 500 Internal Server Errors, this unexpected response was in an HTML file defining a simple Holberton WordPress site.

DEBUGGING PROCESS
Mr. James, a popular bug debugger, quickly encountered the problem when opening the project, which was instructed to rebuild it, around 7:20 P.M., still Nigerian time.  However, he was quickly set about solving it.

While checking, it was discovered that the running processes using ps aux, two apache2 processes - root and www-data - were properly running at the same time.
We looked in the sites-available folder in the /etc/apache2/ directory. The web server intended to serve the content is located at /var/www/html/.
In one terminal, points to the root Apache process, PID was running strace, and in another, a curled server, in which nice tidies were expected, only to be disappointed in the fact that, the strace wasn’t giving any useful information regarding the incident.
Step 3 is repeated, except in the process of PID www-data. Lower expectations this time...but rewarded! strace indicates -1 ENOENT (no such file or directory) error was occurring when trying to access the file /var/www/html/wp-includes/class-wp-locale.phpp.
I looked at the files in the /var/www/html/ directory one by one, trying to match the Vim patterns to the Errors .php file extension. Located in the wp-settings.php file. (Line 137, require once (ABSPATH.WPINC.'/class-wp-locale.php');).
Trailing p removed from the line.
Another loop was tested on the server. It was 200 A-ok!
Puppet obviously wrote to automate the error fixed. 

SUMMATION
The summation is that, the WordPress app while running encountered a critical error in a certain file from the wp-settings with .php extention. This means that, when the server tried to load the class-wp-locale.php, the accurate process in the wp-content directory of the application folder.

Trailing p was removed from the line and the patch includes a simple fix on the typo in the process.

PREVENTION
From all indications and investigations, the error didn’t cause by a web server, but it was by web application software. To prevent this from future occurrences, the following steps below should be observed regularly.

The application should be tested before arraying. The incident wouldn’t have occurred and could equally be prevented if this step was tested earlier.

The status monitoring system should be developed to ensure that some uptime-monitoring services such as Up-time-Robot for instant alertness should this error try to pop up on the website.

