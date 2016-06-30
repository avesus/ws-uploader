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

## Terms
### Controlling server
Controlling server is usually the same where you serve your REST APIs.
Clients publish information about data they have on controlling server.
Small messages can go right thru controlling server but information about
large data only shared with controlling server. Sending is supposed thru a network of upload servers.
When controlling server needs access to that information,
it sends special request to client to ask about transmitting that information (specifying parts of it)
and describing targets - to which upload servers to send the data.

### Upload server
Upload server is a server capable to receive HTTP and WebSocket connections thru Data Upload Protocol (TODO: specify the protocol).
Upload servers even can be hosted over NATs and their TCP connection endpoint be resolving with means of controlling server.


### Client
Browser code, user of this library. Interacts with user, controlling server and upload servers.

## Uploading techniques
Data in buffer memory can be uploaded both thru controlling server or upload server
by means of AJAX request or WebSocket message.

Very small file can be uploaded thru multipart/form-data via controlling server.

Medium and large files can be uploaded using browser's File API and uploaded part-by-part
using WebSockets when it is necessary to control downloading process.
In some cases it can be chosen to start uploading with multipart/form-data
and if controlling server receives HTTP Byte Serving request from uploading server,
uploading via multipart/form-data can break and over WebSockets will started.

## Data categories
### Short message
Pretty short messages or very small images which can be sent
to downloading side right thru controlling server bypassing upload servers.

### Text or small image clipboard content
Can be published and stored as a whole on upload servers.
Data with size up to 500 MB.
Can be downloaded by attaching base64 image content / file data instead of `<img>` tag
or by parts controlled not by HTTP Byte Serving but by WebSockets messaging to receive chunks
of data on downloading client side and assembling them there. Up to not-very-large files
which can be downloaded into modern stores locally in browser settings/cached data folder.
Good for going offline after receiving downloaded files.

### Large file
These files can't be stored in browser's cache on downloading side and can be accessed from
video players with HTTP Byte Serving protocol allowing ff/rewind operations.

# Idea
The idea behind this library is to allow to upload files and data simultaneously and robustly over multiple servers.
1. Browser client sends ('publishes') information about information it has to the controlling server.
Controlling server is usually the same where you serve your REST APIs.
These information are large text snippets and data from clipboard.
Short messages are supplied directly to controlling server.
2. Controlling server supplies client with information about available upload servers.
Client connects to all of them to maintain connection.
3. Contolling server sends request to client about which part of information it needs to transmit
to which upload server.

