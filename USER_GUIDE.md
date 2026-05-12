# Meshtastic Node Map Builder and Analyzer V 2.0 User Guide

Meshtastic Node Map Builder and Analyzer V 2.0 is a browser based utility designed to help users observe what a locally connected Meshtastic node is able to see.

It reads data from a Meshtastic device connected through USB serial, analyzes the observed stream, builds a local node table, displays detected positions on a map, shows radio values, provides a compact local chat interface, and allows the user to save or reload an analysis session in JSON format.

The tool is not a cloud relay, not a hosted messaging platform, and not a replacement for the official Meshtastic applications.

The website hosts the page.

The browser runs the code.

The Meshtastic node is connected locally to the user's computer.

The serial connection is opened only after explicit browser permission.

The server does not receive, forward, or store Meshtastic message contents.

## Purpose of this tool

The purpose of this project is to make local Meshtastic observation easier.

A Meshtastic node can receive different kinds of information from the mesh: node IDs, names, positions, telemetry, packet information, radio values, route related data, traceroutes, and text messages.

This tool tries to collect those elements when they are visible through the serial stream and present them in a more readable way.

It can be useful for radio tests, antenna experiments, coverage checks, local mapping, packet observation, and understanding what is happening from the point of view of one connected node.

The tool does not provide a complete view of the whole mesh.

It only shows what the local node is able to expose through the serial connection.

## Requirements

To use the tool you need:

- A Meshtastic compatible device connected by USB
- A USB data cable, not a charge only cable
- A browser with Web Serial support
- A secure web context, usually HTTPS or localhost
- A recognized serial port on your operating system
- A configured Meshtastic node with region and channel already set

Chromium based browsers such as Chrome, Edge, Chromium, or ChromeOS Chromium are the most suitable choice.

Firefox and Safari are not recommended for this tool because Web Serial support is not available in the same way.

## Supported connection method

This tool uses Web Serial.

The browser asks the user to choose a serial port.

After the user grants permission, the page can communicate with the connected Meshtastic device.

The connection is local.

The browser talks to the serial device attached to the computer.

If the page is refreshed, closed, or reopened later, the serial connection must be started again.

This is expected behavior.

A browser page should not silently reopen hardware access without the user being involved.

## Before starting

Before opening the tool, make sure your Meshtastic device is working normally.

It is usually better to configure the device first with the official Meshtastic app, web client, or CLI.

At minimum, the device should already have the correct region, modem preset, and channel configuration.

The tool is designed for observation and local analysis.

It is not meant to be the first tool used to flash or initially configure a new device.

## Opening the tool

Open the online page from a supported browser.

Connect the Meshtastic device to your computer using a proper USB data cable.

Click the button used to connect to the serial port.

When the browser shows the device selection dialog, choose the serial port associated with your Meshtastic node.

After the connection is opened, the serial log should start appearing in the page.

If no data appears immediately, wait a little.

Some information is only printed when the node receives packets, sends status lines, or produces log output.

## Choosing the correct serial port

On Windows, the device is usually visible in Device Manager under:

~~~text
Ports (COM & LPT)
~~~

Common names include:

~~~text
Silicon Labs CP210x USB to UART Bridge (COMx)
USB Serial Device (COMx)
~~~

On Linux, the port is usually visible as:

~~~text
/dev/ttyUSB0
/dev/ttyACM0
~~~

On macOS, it may appear as:

~~~text
/dev/cu.SLAB_USBtoUART
/dev/cu.usbserial
/dev/cu.usbmodem
~~~

The exact name depends on the board, USB chip, driver, and operating system.

## Driver notes

Some devices require USB serial drivers.

Many Heltec, T-Beam, and ESP32 based boards use Silicon Labs CP210x USB to UART chips.

Some cheaper boards or clones may use CH340 or CH341 chips.

If the browser does not show the device, check whether the operating system recognizes the board correctly before troubleshooting the web tool.

On Linux, the user may need serial port permissions.

A common command is:

~~~bash
sudo usermod -a -G dialout $USER
~~~

After changing group permissions, log out and log back in.

On some Linux systems, ModemManager can interfere with serial devices.

If the port is detected but cannot be opened, check whether ModemManager is grabbing the device.

If Chromium is installed as a Snap package, USB access may require an additional permission connection:

~~~bash
sudo snap connect chromium:raw-usb
~~~

Depending on the system, loading serial modules may also help:

~~~bash
sudo modprobe usbserial
sudo modprobe cp210x
~~~

## Main screen overview

After connection, the page starts reading the serial stream.

The interface is organized around several working areas:

- Connection and serial controls
- Live log output
- Detected nodes
- Map view
- Radio values
- Traceroute and route related information
- Local Meshtastic chat
- Export and session controls

The exact amount of information shown depends on what the local node receives and exposes.

## Detected nodes

The detected nodes section lists nodes observed through the serial stream.

When available, the table can show:

- Node ID
- Long name
- Short name
- Position
- RSSI
- SNR
- Packet counters
- Estimated radio quality
- Last seen information
- Available actions

Not every node will have all fields populated.

A node may appear only with an ID at first.

Later, if more packets are received, the tool may update the entry with a name, position, radio values, or additional metadata.

The tool does not invent missing values.

It updates the local record when new useful data becomes available.

## Map behavior

The map is based on Leaflet.

It may start centered on Italy, but it is not limited to Italy.

If the connected Meshtastic node receives valid coordinates from nodes in another geographic area, the map can display those nodes as well.

Markers are created only when valid coordinates are available.

This means that a node without position data can appear in the node table but not on the map.

The map should be understood as a local reconstruction based on the information observed by the connected node, not as a complete public map of the Meshtastic network.

## Building your own personal map

The idea behind the personal map is simple: your node becomes the local point of observation.

As your node receives information from the mesh, the tool collects useful data and builds a map from what has been observed.

The result depends on your radio environment.

A better antenna, a better position, less obstruction, correct regional settings, and active nodes nearby can all change what your local node is able to see.

The map is not downloaded from a central Meshtastic database.

It is built from the data received during your local session.

If the session is saved, the observed state can be reloaded later from a JSON file.

## Radio values

When available, the tool extracts values such as RSSI and SNR.

RSSI gives an indication of received signal power.

SNR gives an indication of signal quality compared to noise.

These values should not be read in isolation.

A node with a weaker RSSI can still be usable if SNR is acceptable.

A strong signal can still be problematic if the noise level is high or if the radio environment is unstable.

The values shown by the tool are observations from the local node's point of view.

## Traceroute and route information

The tool can observe route and traceroute related information when it appears in the serial stream.

Traceroute data can help understand how packets may be moving through the mesh.

This does not mean that every route is always visible or that the same path will always be used.

Meshtastic routing behavior can change depending on node availability, hop limits, firmware behavior, channel settings, and radio conditions.

If you click traceroute or info related actions, wait before repeating the same action.

Sending repeated requests too quickly can add unnecessary traffic to the mesh and may not produce better results.

A practical waiting time between one info or traceroute request and the next is at least 60 seconds.

## Local Meshtastic chat

Version 2.0 includes a compact local Meshtastic chat section.

The chat is placed after the detected nodes section and is designed as a technical console, not as a cloud messaging service.

It can be used to send messages through the locally connected Meshtastic node.

The chat history is local to the browser session and can be included in a saved JSON session.

The website server does not act as a chat server.

## Sending broadcast messages

A broadcast message is sent to the selected channel.

This is useful for testing channel communication or sending a message to the mesh through the connected node.

Broadcast delivery is not guaranteed.

Whether other nodes receive the message depends on radio coverage, antenna setup, channel configuration, hop limits, duty cycle, routing behavior, congestion, firmware behavior, and whether other nodes are actually listening.

## Sending direct messages

A direct message is sent to a specific Meshtastic node ID.

The expected format is usually:

~~~text
!1234abcd
~~~

The tool validates and normalizes the node ID before writing to the serial port.

If the destination is not valid, the message is not sent.

Direct messages may optionally request ACK when the feature is available in the interface.

## Receiving messages

The tool can show messages observed by the local node.

When possible, it tries to distinguish between:

~~~text
Broadcast messages
Direct messages
Unknown or not clearly classified messages
~~~

This classification depends on the information available in the observed packet or log line.

The local node may not see every message in the mesh.

The page only displays what the connected node exposes through the serial stream.

## ACK handling

ACK handling in this tool is intentionally cautious.

An ACK is not a read receipt.

An ACK does not mean that a human user has read the message.

An ACK should be interpreted only as a technical indication when the protocol response or the node behavior provides enough information.

The tool tries to avoid false positives by checking correlation with the expected packet ID and by avoiding generic lines that only mention ACK without enough context.

The tool also avoids treating the local echo of a recently sent message as proof of delivery.

This is important because the serial stream can sometimes show the text that was just sent.

That does not automatically mean the final destination received it.

## Saving a session

The session can be saved as a JSON file.

The JSON file is created locally by the browser and downloaded to the user's computer.

There is no upload to the website server.

A saved session can include useful analysis state such as:

- Detected nodes
- Map state
- Logs
- Filtered matches
- RSSI and SNR values
- Message history
- Traceroute information
- Active traceroute state
- Local node information
- Page settings
- Filters
- Notes
- Counters
- Theme and interface state

The exact content depends on what has been collected during the session.

## Loading a session

A previously saved JSON session can be loaded from the user's computer.

The browser reads the file locally and the page rebuilds the saved analysis state.

This is useful when reviewing a previous test, comparing radio conditions, documenting node behavior, or continuing analysis after a browser refresh.

If the file is not compatible or is not a valid session file, it will not be loaded correctly.

## What a saved session cannot do

A saved session cannot restore the live serial connection.

When the page is reopened, the Meshtastic device must be connected again manually.

This is required by the browser permission model.

A JSON file can restore the analysis context, but it cannot automatically reopen hardware access.

## Exporting logs

The tool can export visible or collected log information when the export function is available.

Log exports are useful for later review, debugging, documentation, or sharing technical details without having to keep the browser open.

Be careful before sharing logs publicly.

Logs may contain node IDs, names, positions, messages, or other data that should not be exposed without checking.

## Privacy model

The project follows a local first design.

The browser performs the analysis.

The connected Meshtastic node belongs to the user.

The JSON session file is generated locally.

The server hosts the web page but does not intentionally receive, forward, or store Meshtastic message contents.

Map tiles may be loaded from external map providers as part of the Leaflet map visualization.

This is normal for web maps and is separate from Meshtastic message handling.

Users should still remember that Meshtastic traffic on public or default channels may not be private, especially when known or default keys are used.

## Security notes

Do not use this tool with the assumption that all traffic is private.

Do not share saved sessions without reviewing their content.

Do not publish logs without checking for node IDs, positions, or message text.

Do not rely on ACK as a delivery guarantee.

Do not use repeated traceroute or info requests aggressively.

Respect local radio regulations and Meshtastic community guidance.

## Common problems

### The browser does not show my device

Check that you are using a Chromium based browser.

Check that the USB cable supports data.

Check that the operating system sees the serial device.

Install the correct USB serial driver if required.

Try another USB port.

### The serial port is visible but cannot be opened

Close other programs that may be using the same port.

On Linux, check user permissions for the serial device.

Check whether ModemManager is interfering with the device.

If using Chromium installed as Snap, check raw USB permissions.

### The page connects but no nodes appear

Wait for the local node to receive data.

Check that the Meshtastic device is configured correctly.

Check region, channel, modem preset, and antenna.

Move the node to a better position.

Remember that the tool can only show what the local node receives.

### The map is empty

The map needs valid coordinates.

A node can be detected without exposing a usable position.

Wait for position packets or node information containing coordinates.

Check whether position sharing is enabled on the nodes being observed.

### Messages do not appear to arrive

Check that the local node is connected and configured.

Check that the channel index is correct.

Check that the destination node ID is valid for direct messages.

Check radio coverage and antenna placement.

Do not assume that a sent message is delivered just because it was accepted locally by the node.

### ACK does not appear

ACK may not be available in every situation.

ACK is mainly meaningful for direct messages.

Broadcast messages should not be treated as ACK based communication.

Radio conditions, routing, firmware behavior, and destination availability can affect whether a useful ACK is observed.

## Recommended workflow

Connect the Meshtastic node.

Open the web page in a supported browser.

Authorize the serial port.

Let the node observe the mesh for a while.

Check the detected nodes table.

Open the map and verify which nodes have valid coordinates.

Review RSSI and SNR values where available.

Use traceroute or info actions carefully and not too frequently.

Use the local chat for controlled broadcast or direct message tests.

Save the session as JSON before closing the page.

Reload the JSON later if you want to review the same analysis state.

## Project scope

This tool is designed for local analysis and learning.

It is not intended to replace the official Meshtastic clients.

It is not a full packet decoder for every possible Meshtastic frame.

It is not a central node database.

It is not a delivery guaranteed messaging platform.

It is not a cloud relay.

Its purpose is to help users understand what their own node can see and how that information can be represented in a local browser interface.

## Online version

The online version is hosted by Aiuto Computer Help.

Website:

~~~text
https://www.aiutocomputerhelp.it/meshtastic-node-map-builder-analyzer.php
~~~

Project page:

~~~text
https://www.aiutocomputerhelp.it/en/meshtastic-node-map-builder-and-analyzer-build-your-own-personal-map/
~~~

Manual page:

~~~text
https://www.aiutocomputerhelp.it/en/meshtastic-node-map-builder-and-analyzer-manual/
~~~

## Author

Created and maintained by Giovanni Popolizio / Aiuto Computer Help.

Website:

~~~text
https://www.aiutocomputerhelp.it/
~~~

## License

This project is released under the license included in this repository.

Read the LICENSE file before copying, modifying, redistributing, or hosting the project elsewhere.

## Disclaimer

This software is provided as a technical and educational tool.

Use it responsibly.

The author is not responsible for misuse, radio regulation violations, data exposure caused by shared logs or sessions, wrong configuration, message loss, hardware issues, or unexpected behavior of Meshtastic devices, firmware, browsers, operating systems, drivers, or radio networks.
