Usage Examples
==============

Kick a single Client from a Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // kick the client with ID 123 from the server
    $ts3_VirtualServer->clientKick(123, TeamSpeak3::KICK_SERVER, "evil kick XD");

    // spawn an object for the client by unique identifier and do the kick
    $ts3_VirtualServer->clientGetByUid("FPMPSC6MXqXq751dX7BKV0JniSo=")->kick(TeamSpeak3::KICK_SERVER, "evil kick XD");

    // spawn an object for the client by current nickname and do the kick
    $ts3_VirtualServer->clientGetByName("ScP")->kick(TeamSpeak3::KICK_SERVER, "evil kick XD");

Kick all Clients from a Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // query clientlist from virtual server
    $arr_ClientList = $ts3_VirtualServer->clientList();

    // kick all clients online with a single command
    $ts3_VirtualServer->clientKick($arr_ClientList, TeamSpeak3::KICK_SERVER, "evil kick XD");

Print the Nicknames of connected Android Clients
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // query clientlist from virtual server and filter by platform
    $arr_ClientList = $ts3_VirtualServer->clientList(array("client_platform" => "Android"));

    // walk through list of clients
    foreach($arr_ClientList as $ts3_Client)
    {
        echo $ts3_Client . " is using " . $ts3_Client["client_platform"] . "<br />\n";
    }

Modify the Settings of each Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once ("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the server instance
    $ts3_ServerInstance = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/");

    // walk through list of virtual servers
    foreach($ts3_ServerInstance as $ts3_VirtualServer) {

        // modify the virtual servers hostbanner URL only using the ArrayAccess interface
        $ts3_VirtualServer["virtualserver_hostbanner_gfx_url"] = "http://www.example.com/banners/banner01_468x60.jpg";

        // modify the virtual servers hostbanner URL only using property overloading
        $ts3_VirtualServer->virtualserver_hostbanner_gfx_url = "http://www.example.com/banners/banner01_468x60.jpg";

        // modify multiple virtual server properties at once
        $ts3_VirtualServer->modify(array(
            "virtualserver_hostbutton_tooltip" => "My Company",
            "virtualserver_hostbutton_url" => "http://www.example.com",
            "virtualserver_hostbutton_gfx_url" => "http://www.example.com/buttons/button01_24x24.jpg",
        ));
    }

Create a Privilege Key for a Server Group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // spawn an object for the group using a specified name
    $arr_ServerGroup = $ts3_VirtualServer->serverGroupGetByName("Admins");

    // create the privilege key
    $ts3_PrivilegeKey = $arr_ServerGroup->privilegeKeyCreate();


Modify the Permissions of Admins on each Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the server instance
    $ts3_ServerInstance = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/");

    // walk through list of virtual servers
    foreach($ts3_ServerInstance as $ts3_VirtualServer)
    {
        // identify the most powerful group on the virtual server
        $ts3_ServerGroup = $ts3_VirtualServer->serverGroupIdentify();

        // assign a new permission
        $ts3_ServerGroup->permAssign("b_virtualserver_modify_hostbanner", TRUE);

        // revoke an existing permission
        $ts3_ServerGroup->permRemove("b_virtualserver_modify_maxclients");
    }

Create a new Virtual Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the server instance
    $ts3_ServerInstance = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/");

    // create a virtual server and get its ID
    $new_sid = $ts3_ServerInstance->serverCreate(array(
        "virtualserver_name" => "My TeamSpeak 3 Server",
        "virtualserver_maxclients" => 64,
        "virtualserver_hostbutton_tooltip" => "My Company",
        "virtualserver_hostbutton_url" => "http://www.example.com",
        "virtualserver_hostbutton_gfx_url" => "http://www.example.com/buttons/button01_24x24.jpg",
    ));

Create a hierarchical Channel Structure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php
    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // create a top-level channel and get its ID
    $top_cid = $ts3_VirtualServer->channelCreate(array(
        "channel_name" => "My Channel",
        "channel_topic" => "This is a top-level channel",
        "channel_codec" => TeamSpeak3::CODEC_SPEEX_WIDEBAND,
        "channel_flag_permanent" => TRUE,
    ));

    // create a sub-level channel and get its ID
    $sub_cid = $ts3_VirtualServer->channelCreate(array(
        "channel_name" => "My Sub-Channel",
        "channel_topic" => "This is a sub-level channel",
        "channel_codec" => TeamSpeak3::CODEC_SPEEX_NARROWBAND,
        "channel_flag_permanent" => TRUE,
        "cpid" => $top_cid,
    ));

Send a Text Message to outdated Clients
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // connect to default update server
    $ts3_UpdateServer = TeamSpeak3::factory("update");

    // walk through list of clients on virtual server
    foreach ($ts3_VirtualServer->clientList() as $ts3_Client) {
        // skip query clients
        if ($ts3_Client["client_type"]) continue;

        // send test message if client build is outdated
        if ($ts3_Client->getRev() < $ts3_UpdateServer->getClientRev()) {
            $ts3_Client->message("[COLOR=red]your client is [B]outdated[/B]... update to [U]" . $ts3_UpdateServer->getClientVersion() . "[/U] now![/COLOR]");
        }
    }

Check if the Server Instance is Outdated or Blacklisted
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the server instance
    $ts3_ServerInstance = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/");

    // connect to default update server
    $ts3_UpdateServer = TeamSpeak3::factory("update");

    // send global text message if the server is outdated
    if ($ts3_ServerInstance->version("build") < $ts3_UpdateServer->getServerRev()) {
        $ts3_ServerInstance->message("[COLOR=red]your server is [B]outdated[/B]... update to [U]" . $ts3_UpdateServer->getServerVersion() . "[/U] now![/COLOR]");
    }

    // connect to default blacklist server
    $ts3_BlacklistServer = TeamSpeak3::factory("blacklist");

    // send global text message if the server is blacklisted
    if ($ts3_BlacklistServer->isBlacklisted($ts3_ServerInstance)) {
        $ts3_ServerInstance->message("[COLOR=red]your server is [B]blacklisted[/B]... disconnect now![/COLOR]");
    }


Create a simple TSViewer for your Website
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // build and display HTML treeview using custom image paths (remote icons will be embedded using data URI sheme)
    echo $ts3_VirtualServer->getViewer(new TeamSpeak3_Viewer_Html("images/viewericons/", "images/countryflags/", "data:image"));

Update all outdated Audio Codecs to their Opus equivalent
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // walk through list of chanels
    foreach ($ts3_VirtualServer->channelList() as $ts3_Channel) {
        if ($ts3_Channel["channel_codec"] == TeamSpeak3::CODEC_CELT_MONO) {
            $ts3_Channel["channel_codec"] = TeamSpeak3::CODEC_OPUS_MUSIC;
        } elseif ($ts3_Channel["channel_codec"] != TeamSpeak3::CODEC_OPUS_MUSIC) {
            $ts3_Channel["channel_codec"] = TeamSpeak3::CODEC_OPUS_VOICE;
        }
    }

Display the Avatar of a connected User
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php
    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // spawn an object for the client using a specified nickname
    $ts3_Client = $ts3_VirtualServer->clientGetByName("John Doe");

    // download the clients avatar file
    $avatar = $ts3_Client->avatarDownload();

    // send header and display image
    header("Content-Type: " . TeamSpeak3_Helper_Convert::imageMimeType($avatar));
    echo $avatar;

Create a Simple Bot waiting for Events
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // connect to local server in non-blocking mode, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987&blocking=0");

    // get notified on incoming private messages
    $ts3_VirtualServer->notifyRegister("textprivate");

    // register a callback for notifyTextmessage events
    TeamSpeak3_Helper_Signal::getInstance()->subscribe("notifyTextmessage", "onTextmessage");

    // wait for events
    while (1) {
        $ts3_VirtualServer->getAdapter()->wait();
    }

    // define a callback function
    function onTextmessage(TeamSpeak3_Adapter_ServerQuery_Event $event, TeamSpeak3_Node_Host $host)
    {
        echo "Client " . $event["invokername"] . " sent textmessage: " . $event["msg"];
    }

Handle Errors using Exceptions and Custom Error Messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // register custom error message (supported placeholders are: %file, %line, %code and %mesg)
    TeamSpeak3_Exception::registerCustomMessage(0x300, "The specified channel does not exist; server said: %mesg");

    try {
        // connect to local server, authenticate and spawn an object for the virtual server on port 9987
        $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

        // spawn an object for the channel using a specified name
        $ts3_Channel = $ts3_VirtualServer->channelGetByName("I do not exist");
    } catch (TeamSpeak3_Exception $e) {
        // print the error message returned by the server
        echo "Error " . $e->getCode() . ": " . $e->getMessage();
    }

Save Connection State in Persistent Session Variable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // start a PHP session
    session_start();

    // connect to local server, authenticate and spawn an object for the virtual server on port 9987
    $ts3_VirtualServer = TeamSpeak3::factory("serverquery://username:password@127.0.0.1:10011/?server_port=9987");

    // save connection state (including login and selected virtual server)
    $_SESSION["_TS3"] = serialize($ts3_VirtualServer);

Restore Connection State from Persistent Session Variable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: php

    <?php

    // load framework files
    require_once("libraries/TeamSpeak3/TeamSpeak3.php");

    // start a PHP session
    session_start();

    // restore connection state
    $ts3_VirtualServer = unserialize($_SESSION["_TS3"]);

    // send a text message to the server
    $ts3_VirtualServer->message("Hello World!");
