# Meshtastic-Node-Map-Builder-and-Analyzer-V-2.0
Browser based Meshtastic utility using Web Serial to analyze local node data, map detected nodes, inspect RSSI and SNR, send broadcast or direct messages, and save or reload JSON sessions without server side message storage.
# Meshtastic Node Map Builder and Analyzer V 2.0

Meshtastic Node Map Builder and Analyzer is a browser based utility for observing and analyzing data received from a locally connected Meshtastic node.

The tool works through the Web Serial API and allows the browser to communicate with a Meshtastic device connected to the user’s computer. It can read serial output, extract useful information from the observed stream, build a node table, display detected positions on a map, inspect radio values, observe messages, run basic node related actions, and save or reload a local analysis session.

This project is not a cloud relay, not a centralized messaging platform, and not a remote Meshtastic gateway. The web page is hosted online, but the actual communication happens locally between the browser and the connected Meshtastic device, after explicit user permission.

The server hosting the page does not receive, forward, or store Meshtastic message contents.

## What it does

Meshtastic Node Map Builder and Analyzer helps users understand what their local node is able to observe on the mesh.

It can collect and display information such as detected node IDs, names, short names, positions, packet counters, RSSI, SNR, estimated radio quality, telemetry values, MQTT references when visible, routes, traceroutes, neighbors, and text messages observed through the connected node.

The map starts centered on Italy, but it is not limited to Italy. If the connected node receives valid coordinates from nodes in other geographic areas, those nodes can be displayed as well.

The tool is intended for analysis, testing, learning, and local observation of Meshtastic behavior.

## Main features

- Local connection to a Meshtastic node through Web Serial
- Serial log reading directly inside the browser
- Node detection and local node table
- Map visualization with Leaflet
- RSSI and SNR extraction when available
- Position and telemetry observation when present in the data stream
- Traceroute and route related analysis when visible
- Local Meshtastic chat inside the page
- Broadcast message sending on the selected channel
- Direct message sending to a specific node ID
- Reception of observed broadcast and direct messages
- Cautious ACK handling for direct messages
- Local JSON session save
- Local JSON session reload
- Local TXT export of visible log data

## New in V 2.0

Version 2.0 adds a more complete local workflow.

The page now includes a compact Meshtastic chat section placed after the detected nodes area. This chat can send broadcast messages on a selected channel and can send direct messages to a specific Meshtastic node ID.

The receive logic has also been improved. When possible, the tool tries to distinguish between broadcast messages and direct messages observed by the local node.

ACK handling has been made more careful. ACK is not presented as a read receipt and is not treated as a guaranteed delivery confirmation. It is interpreted as a technical indication when the protocol response or node behavior provides enough information.

The code also avoids a common false positive: the local echo of a recently sent message. A message that appears again in the serial stream does not automatically mean that the final recipient received it.

Version 2.0 also introduces local session save and load in JSON format. This allows users to save the state of an analysis session and reload it later without sending the session to a remote server.

## Local first design

This tool follows a local first approach.

The website hosts the page.  
The browser runs the JavaScript code.  
The user explicitly authorizes access to the serial port.  
The connected Meshtastic node sends and receives over radio.  
The session file is generated locally and downloaded by the browser.  

The server does not act as a relay and does not store message contents.

## What is saved in a JSON session

A saved session can include the useful state needed to reconstruct an analysis session, such as detected nodes, map state, logs, filtered matches, radio values, local messages, traceroute information, local node information, counters, filters, notes, theme, and page settings.

The saved JSON file is created by the browser and downloaded to the user’s computer.

No session archive is created on the server.

## What is not saved

The live serial connection is not saved.

When the page is reopened, the user must reconnect the Meshtastic device manually. This is required because browser access to serial ports is permission based and cannot be silently restored from a JSON file.

The JSON session can restore the analysis context, but it cannot reopen hardware access automatically.

## Browser requirements

This tool requires a browser with Web Serial support.

It is mainly intended for Chromium based browsers such as Chrome, Edge, Chromium, and other compatible browsers.

Firefox and Safari do not currently provide the same Web Serial support required by this project.

The page must be served from a secure context, such as HTTPS or localhost, for Web Serial to work correctly.

## Hardware requirements

A Meshtastic compatible device connected to the computer through USB serial is required.

The operating system must recognize the device as a serial port.

Depending on the board and operating system, drivers may be required. For example, some devices use Silicon Labs CP210x USB to UART drivers.

On Linux, the user may also need permission to access the serial port, usually by being part of the correct system group such as dialout.

## Important notes about messaging

The chat feature sends messages through the locally connected Meshtastic node.

Broadcast messages are sent to the selected channel and should not be interpreted as guaranteed delivery.

Direct messages require a valid Meshtastic node ID.

ACK, when requested and available, should be interpreted only as a technical confirmation. It is not a read receipt and does not guarantee that a human user has read the message.

Radio communication depends on many factors, including coverage, antenna setup, node configuration, channel settings, hop limits, routing behavior, firmware version, and the presence of other nodes.

## Privacy notes

The tool is designed to avoid server side message handling.

Message contents handled by the local chat are not intentionally sent to the website server by the application logic.

Saved sessions are generated locally as JSON files and downloaded by the browser.

The map uses external map tiles through Leaflet and OpenStreetMap related infrastructure, as normally happens with web maps. This concerns map visualization, not Meshtastic message relay.

Users should still be aware that Meshtastic traffic on public or default channels may not be private, especially when default channel keys are used.

## Limitations

This tool only sees what the connected local node exposes through the serial stream.

It cannot provide a complete view of the entire mesh.

It cannot guarantee message delivery.

It cannot guarantee that all packet types will be decoded in every firmware version.

It cannot automatically restore a serial connection from a saved session.

It is an analysis and observation tool, not a replacement for the official Meshtastic applications.

## Intended use

Meshtastic Node Map Builder and Analyzer is intended for users who want to study and better understand the behavior of their Meshtastic node and the surrounding mesh.

It can be useful for radio tests, antenna experiments, mapping observed nodes, comparing RSSI and SNR values, documenting traceroutes, checking local reception, and learning how Meshtastic traffic appears from the point of view of a connected node.

## Online version

The online version is available at:

https://www.aiutocomputerhelp.it/

Project page:

Meshtastic Node Map Builder and Analyzer V 2.0

## Author

Created and maintained by Aiuto Computer Help.

Website:

https://www.aiutocomputerhelp.it/

## License

Copyright © 2026 Giovanni Popolizio / Aiuto Computer Help. All rights reserved.

Use of the software must respect the license terms, the Meshtastic project guidelines, local radio regulations, and applicable laws.

