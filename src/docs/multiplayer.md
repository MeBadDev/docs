<!--
MIT License

Copyright (c) 2020-2022 Macaroni Studios AB

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
-->

### Networked multiplayer

On Gotm your players can connect to each other using [NetworkedMultiplayerENet](https://docs.godotengine.org/en/stable/classes/class_networkedmultiplayerenet.html) or [PacketPeerUDP](https://docs.godotengine.org/en/stable/classes/class_packetpeerudp.html) without plugins or port-forwarding.
Players connect through a unique invitation link that is generated when a player hosts a game. Try it out!

<Game src="game-examples/networkedmultiplayerenet" />

_Press "Host" to generate an invitation link. Join the game by entering the link in a new browser window and pressing "Join"._

<HorizontalList>
  <ExternalLink href="https://github.com/PlayGotm/Game-Examples/releases/latest/download/NetworkedMultiplayerENet.zip">
    <OutlinedButton padding="10px" margin="10px">
      <FiDownload size="24px" />
      <span>Download project</span>
    </OutlinedButton>
  </ExternalLink>
  <ExternalLink href="https://github.com/PlayGotm/Game-Examples/tree/master/NetworkedMultiplayerENet">
    <OutlinedButton padding="10px" margin="10px">
      <FiCode size="24px" />
      <span>Browse source</span>
    </OutlinedButton>
  </ExternalLink>
</HorizontalList>

The above game example uses NetworkedMultiplayerENet to connect players in a shared drawing session.
See the [source code](https://github.com/PlayGotm/Game-Examples/tree/master/NetworkedMultiplayerENet) for more details.

<Anchor id="get-started"></Anchor>

#### Get started

You can quickly get started by downloading the [example project](https://github.com/PlayGotm/Game-Examples/releases/latest/download/NetworkedMultiplayerENet.zip).
The [web player](/web-player) is recommended for testing your game on Gotm without having to sign in.

<Anchor id="1-generate-invitation-link"></Anchor>

##### 1. Generate invitation link

When you call [NetworkedMultiplayerENet.create_server](https://docs.godotengine.org/en/stable/classes/class_networkedmultiplayerenet.html#class-networkedmultiplayerenet-method-create-server) or [PacketPeerUDP.listen](https://docs.godotengine.org/en/stable/classes/class_packetpeerudp.html#class-packetpeerudp-method-listen) Gotm automatically generates an invitation link.
The link is shown in a popup, so it can easily be copied and shared with other players.

```
var peer = NetworkedMultiplayerENet.new()
peer.create_server(8070)
get_tree().set_network_peer(peer)
```

<Anchor id="2-accept-invitation"></Anchor>

##### 2. Accept invitation

Accept the invitation by entering the link in another browser window.
You can now connect to the host using [NetworkedMultiplayerENet.create_client](https://docs.godotengine.org/en/stable/classes/class_networkedmultiplayerenet.html#class-networkedmultiplayerenet-method-create-server) or with your custom PacketPeerUDP code.

```
var peer = NetworkedMultiplayerENet.new()
peer.create_client("127.0.0.1", 8070)
get_tree().set_network_peer(peer)
```

<Anchor id="3-find-the-host"></Anchor>

##### 3. (Optional) Find the host

All players that have accepted the invitation have joined a shared virtual local network.
This allows your game to send and receive RPC commands and packets to these players using methods like [Node.rpc](https://docs.godotengine.org/en/stable/classes/class_node.html?#class-node-method-rpc) or [PacketPeerUDP.put_packet](https://docs.godotengine.org/en/stable/classes/class_packetpeer.html#class-packetpeer-method-put-packet). You can read more about Godot's networking and RPC commands [here](https://docs.godotengine.org/en/stable/tutorials/networking/high_level_multiplayer.html).

The easiest way to connect to the host is by using the special `127.0.0.1` address:

```
var peer = NetworkedMultiplayerENet.new()
peer.create_client("127.0.0.1", 8070)
```

This address always points to the player that created the invitation link. If you want to manually discover hosts with custom PacketPeerUDP broadcasting, that works too:

```
var peer = PacketPeerUDP.new()
peer.set_broadcast_enabled(true) ### Only needed for 3.2 or later.
peer.set_dest_address("255.255.255.255", 8070)
peer.put_var("Sent to all players in network.")
```

_Note: To send a packet to yourself with PacketPeerUDP you can use `localhost` instead of `127.0.0.1`._