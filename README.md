# ws-uploader
Server-controlled files uploader over WebSockets.
Supports uploading of arbitrary file parts and splitting of uploads between multiple servers.

# Description
The module is browser side code.
## Init
Supplied with server addresses and authentication means. Servers can operate different versions of the upload protocol.
## Upload
Controlling server supplies ws-uploader with information of which files and parts of them to upload
and to which upload servers (specified in initialization).

