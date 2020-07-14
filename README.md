# Purpose

ALSS is designed to assist in livestreaming church services,
though it could be used for other purposes as well.

It will monitor a streaming service (e.g. YouTube) for pre-scheduled livestreams.
When the time for one arrives,
ALSS will automatically set up the livestream in StreamLabs OBS.
The user will be able to control the livestream through a simple interface
(e.g. an El Gato StreamDeck).
The intention is to allow a busy, non-technical user
(e.g. a church organist) to run a livestream as easily as possible.

# Installation

To be added when we have something to install. :-)

# Usage

TBD

# For Developers

## Architecture

### Stream States

ALSS handles only a single stream at a time.
The overall system state consists of an identifier for the current stream
and a state for that stream.

A `StreamIdentifier` consists of:
* what service the stream belongs to
* a service-specific identifier ("key")

There are five possible `StreamState`s:
* `prebroadcast`: this is prior to the main event being broadcast.  Neither audio nor video will stream.
* `prelude`: this is prior to the main event being broadcast.  Audio will stream but video will not. 
* `live`: audio and video are streaming.  
* `postlude`: this is after the main event being broadcast. Audio will stream but video will not.
* `error`: something has gone wrong

The `StreamIdentifier` and the `StreamState` together make up the `ALSSModel`.

Movement between the states and communication with external interfaces (below)
is controlled by the `ALSSController`.

### External Interfaces

There are three key interfaces for the interaction with ALSS with external things:
- `StreamingSoftware` controls ALSS's interaction with the software which does the actual livestreaming.
Initially our only implementation is `StreamLabsOBSStreamingSoftware`.
- `StreamStateUI` controls ALSS's interaction with the end user for purposes of starting the stream,
stopping the stream, streaming only the audio without video, etc.
Initially our only implementation is `StreamDeckStreamStateUI` for use with an El Gato StreamDeck.
- `StreamingService` controls ALSS's interaction with the service used for streaming.
For now, this interacton is limited to finding out what streams are scheduled
and what their keys are.
Initially, our only implementation is `YouTubeStreamingService`.


