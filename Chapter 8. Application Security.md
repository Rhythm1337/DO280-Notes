#  Control Application Permissions with Security Context Constraints

## What you will learn
Create service accounts, apply permissions and manage security constraints

## Security Context Constraints (SCCs)
SCCs are a security mechanism that limits the access from a running pod in Open Shift to host enviroment. The SCCs control the following:
- Running privileged containers
- Change User ID
- Changing the SELinux context of a container
- Request extra capabilities for a container
- Use host directory as volumes

