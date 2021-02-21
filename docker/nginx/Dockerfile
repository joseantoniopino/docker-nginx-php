FROM nginx:1.19

# Get UID argument and expose to exterior. This argument is a user_id
ARG UID
EXPOSE $UID

# Create user "appuser" inside of container with user id
RUN adduser -u ${UID} --disabled-password --gecos "" appuser

# Copy default.conf file insde of container
COPY default.conf /etc/nginx/conf.d/