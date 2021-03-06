Image uploads use this htaccess

# Disable any cgi-scripts and prevent directory browsing
Options -ExecCGI -Indexes

# Whitelist the following file extensions
# This includes the blocking of double extensions using [^.]
Order Allow,Deny
<FilesMatch "^[^.]+\.(?i:jpe?g|png|gif)$">
Allow from all
</FilesMatch>

# Secure MIME-types
<FilesMatch "\.[Jj][Pp][Ee]?[Gg]$">
ForceType image/jpeg
</FilesMatch>
<FilesMatch "\.[Pp][Nn][Gg]$">
ForceType image/png
</FilesMatch>
<FilesMatch "\.[Gg][Ii][Ff]$">
ForceType image/gif
</FilesMatch>

# Make sure mod_rewrite is running
RewriteEngine On

# Disable scripts
RewriteRule !^(?:[^.]+\.(?:jpe?g|png|gif)|php\.ini)$ - [H=cgi-script,NC,L]

# Only allow the following direct access to the uploads directory
RewriteCond %{REMOTE_ADDR} !^(?:xxx\.xxx\.xxx\.xxx)
RewriteCond %{HTTP_HOST} !^localhost$ [NC]
RewriteCond %{HTTP_REFERER} !^https?://(?:[^.]+\.)?example\.com/ [NC]
RewriteRule .? https://meetingplanner.io [L]

# Disable hotlinking of images
RewriteCond %{REQUEST_FILENAME} -f
RewriteCond %{REQUEST_FILENAME} \.(?:jpe?g|png|gif)$ [NC]
RewriteCond %{HTTP_REFERER} !^(?:https?://(?:[^.]+\.)?example\.com/|$) [NC]
RewriteRule \.(?:jpe?g|png|gif)$ - [NC,F]

# Only allow GET and POST request methods
<LimitExcept GET POST>
Deny from all
</LimitExcept>
