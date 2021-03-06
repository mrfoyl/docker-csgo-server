## Counter Strike Global Offensive with Ebot integration
## All Credit goes to Gonzih, and his csgo docker build on witch this is based on.

CS:GO server in docker with 128 tick enabled by default.

### Docker hub image

```shell
docker pull mrfoyl/docker-csgo-server
```
### Ebot integration (by Mrfoyl)

```shell
Download the "docker-compose.yml" file and do the following to that file
  2. and add local IP of docker host.
  3. Go to http://steamcommunity.com/dev/managegameservers and register for a Game login token.
  4. Add token to docker-compose file - +sv_setsteamaccount 1233456
  
Install docker-compose
Run "docker-compose up" or "docker-compose up -d" and wait for 17 GB download and extraction. CSGO server, ebot, ebotweb and mysql will be pulled, built and run.
  
Go to https://dockerhost:8080/admin.php
  1. log in with default password or the one you set in docker-compose
  2. Add game server with static container ip 10.5.0.5:27015
  3. Save and create match.
  
Kill the enemy!
```

### Details:
By default image is build with enabled autoupdate feature (take a look at `csgo.sh` file).
You can create new Dockerfile based on that image (FROM csgo) and customize it with plugins, configs, CMD and ENTRYPOINT instructions.

```shell
# Build image and tag it as csgo
docker build -t csgo github.com/mrfoyl/docker-csgo-server

# Run image with default options (CMD in Dockerfile)
docker run -d -p 27015:27015 -p 27015:27015/udp csgo

# Run image with as Classic Casual server
docker run -d -p 27015:27015 -p 27015:27015/udp csgo -console -usercon +game_type 0 +game_mode 0 +mapgroup mg_active +map de_cache

# Run image with as Classic Competetive server
docker run -d -p 27015:27015 -p 27015:27015/udp csgo -console -usercon +game_type 0 +game_mode 1 +mapgroup mg_active +map de_cache

# Run image with as Arm Race server
docker run -d -p 27015:27015 -p 27015:27015/udp csgo -console -usercon +game_type 1 +game_mode 0 +mapgroup mg_armsrace +map ar_shoots

# Run image with as Demolition server
docker run -d -p 27015:27015 -p 27015:27015/udp csgo -console -usercon +game_type 1 +game_mode 1 +mapgroup mg_demolition +map de_lake

# Run image with as Deathmatch server
docker run -d -p 27015:27015 -p 27015:27015/udp csgo -console -usercon +game_type 1 +game_mode 2 +mapgroup mg_allclassic +map de_dust

# To run lan server just add `+sv_lan 1` at end of command
docker run -d -p 27015:27015 -p 27015:27015/udp csgo -console -usercon +game_type 0 +game_mode 1 +mapgroup mg_active +map de_cache +sv_lan 1
```

### Running public server

To run public server you need to [Register Login Token](http://steamcommunity.com/dev/managegameservers) and adding `+sv_setsteamaccount THISGSLTHERE -net_port_try 1` to the server command.
Refer to [Docs](https://developer.valvesoftware.com/wiki/Counter-Strike:_Global_Offensive_Dedicated_Servers#Registering_Game_Server_Login_Token) for more details.
