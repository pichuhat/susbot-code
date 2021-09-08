[README.md](https://github.com/pichuhat/susbot-code/files/7132146/README.md)
# susbot-code

# commands.js

/**
 * Commands
 * Cassius - https://github.com/sirDonovan/Cassius
 *
 * This file contains the base commands for Cassius.
 *
 * @license MIT license
 */

'use strict';

// Users who use the settour command when a tournament is already
// scheduled will be added here and prompted to reuse the command.
// This prevents accidentally overwriting a scheduled tournament.
/**@type {Map<string, string>} */
let overwriteWarnings = new Map();

var hostable = 0
var hoster
var newapply = "none"
var devlist = Config.developers

/**@type {{[k: string]: Command | string}} */
let commands = {
	// Developer commands
	js: 'eval',
	eval: function (target, room, user) {
		if (!user.isDeveloper()) return;
		try {
			target = eval(target);
			this.say(JSON.stringify(target));
		} catch (e) {
			this.say(e.name + ": " + e.message);
		}
	},

	// General commands
    
    about: function (target, room, user) {
		if (room instanceof Users.User) {
		return this.say("Hi, i'm " + Config.username + "! I'm a bot. My command character is ``.``. For help, pm user pichuhat. I am based off of sirDonovan's bot system, Cassius. you can see it here: https://github.com/sirDonovan/Cassius");
        } else {
        console.log("isdeveloper", user.isDeveloper())
        if (!user.isDeveloper()) return;
        return this.say("Hi, i'm " + Config.username + "! I'm a bot. My command character is ``.``. For help, pm user pichuhat. I am based off of sirDonovan's bot system, Cassius. you can see it here: https://github.com/sirDonovan/Cassius")  
       }
    },
    
	help: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper()) return;
        console.log("we made it!")
		return this.say("/adduhtml susbothelp, <a href='https://github.com/sirDonovan/Cassius'>susbot's guide</a>");
	},
	mail: function (target, room, user) {
		if (!(room instanceof Users.User) || !Config.allowMail) return;
		let targets = target.split(',');
		if (targets.length < 2) return this.say("Please use the following format: .mail user, message");
		let to = Tools.toId(targets[0]);
		if (!to || to.length > 18 || to === Users.self.id || to.startsWith('guest')) return this.say("Please enter a valid username");
		let message = targets.slice(1).join(',').trim();
		let id = Tools.toId(message);
		if (!id) return this.say("Please include a message to send.");
		if (message.length > (258 - user.name.length)) return this.say("Your message is too long.");
		let database = Storage.getDatabase('global');
		if (to in database.mail) {
			let queued = 0;
			for (let i = 0, len = database.mail[to].length; i < len; i++) {
				if (Tools.toId(database.mail[to][i].from) === user.id) queued++;
			}
			if (queued >= 3) return this.say("You have too many messages queued for " + Users.add(targets[0]).name + ".");
		} else {
			database.mail[to] = [];
		}
		database.mail[to].push({time: Date.now(), from: user.name, text: message});
		Storage.exportDatabase('global');
		this.say("Your message has been sent to " + Users.add(targets[0]).name + "!");
	},
    
    	startuno: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper()) return;
        console.log("we made it!")
		this.say("/uno create 5")
        console.log (user)
        return this.say("/wall A game of uno was started by " + user.name + ".");
        },
    
    	enduno: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper()) return;
        console.log("we made it!")
		this.say("/uno end")
        return this.say("/wall the game of uno was forcibly ended by " + user.name + ".");
        },
    
    	beginuno: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper()) return;
        console.log("we made it!")
		this.say("/uno start")
        return this.say("/wall the game of uno is starting!");
        },
    
    //the command below is under construction.
        basecode: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper()) return;
        console.log("we made it!")
            return this.say("/adduhtml susbothelp, <a href='https://pastebin.com/raw/8aSb1srL'>commands.js</a> <br> <a href='https://pastebin.com/raw/tcq5cMV6'>client.js</a> <br> <a href='https://pastebin.com/raw/zNBbRDLQ'>message.js</a> <br> <a href='https://pastebin.com/raw/CiyxaeeA'>config.js</a> <br> there are more! you'll need them if you're basing your bot off this.");
        },
    
        hostgtp: function (target, room, user) {
		if (room instanceof Users.User) return;
        if (hostable == 1) {
        hoster = user.name
		return this.say("/adduhtml susbothelp, <h1>" + user.name + " is hosting a game of Guess the Pokemon!</h1> <small>" + user.name + "</small>");
         } else {
             return this.say("/sendprivatehtmlbox " + user.name + ", <p>hosting is currently disabled. " + user.name + " please try again later!</p>");
         }
        },
    
        endhostgtp: function (target, room, user) {
		if (room instanceof Users.User) return;
        if (!hoster == user.name) return;
        hoster = "none"
		return this.say("/adduhtml susbothelp, <h1>" + user.name + "'s host of Guess the Pokemon was ended!</h1> <small>" + user.name + "</small>");
        },
    
        potato: function (target, room, user) {
		if (room instanceof Users.User) {
		return this.say("no, its pot - AH - to");
        } else {
        console.log("isdeveloper", user.isDeveloper())
        if (!user.isDeveloper()) return;
        return this.say("no, its pot - AH - to")  
       }
        },
    
        selfclear: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper()) return;
		return this.say("/hidetext pichuhat");
        },
    
        sausage: function (target, room, user) {
		if (room instanceof Users.User) {
		return this.say("no, its sauce - age");
        } else {
        console.log("isdeveloper", user.isDeveloper())
        if (!user.isDeveloper()) return;
        return this.say("no, its sauce - age")
       }
        },
    
        allowhost: function (target, room, user) {
          if (room instanceof Users.User) return;
          console.log("isdeveloper", user.isDeveloper())
          if (!user.isDeveloper()) return;
          hostable = 1
          return this.say("user-hosting is now allowed until ``.disablehost`` is received.");
        },
        disablehost: function (target, room, user) {
          if (room instanceof Users.User) return;
          console.log("isdeveloper", user.isDeveloper())
          if (!user.isDeveloper()) return;
          hostable = 0
          return this.say("user-hosting is now disabled until ``.allowhost`` is received.");
        },
    
        say: function (target, room, user) {
          if (room instanceof Users.User) return;
           this.say("an error occurred. data shown below.")
           return this.say("/adduhtml susboterror, user: " + user.name + " <br> id: " + user.id + " <br> roomId: " + room.id + " <br> clientId: " + room.clientId + " <br> users: " + user.users)
        },
    
        developers: function (target, room, user) {
          if (room instanceof Users.User) {
          return this.say("Current developers: " + Config.developers)  
          } else {
          if (!user.isDeveloper()) return;
          return this.say("Current Developers: " + Config.developers)
          }
        },
    
        apply: function (target, room, user) {
         if (!room instanceof Users.User) return;
         if (newapply == user.name) {
         return this.say("you are already on the apply list!");
         } else {
         if (devlist.includes(user.id)) {
         return this.say("you are already a developer!"); 
          } else {
         if (!newapply == "none") {
         return this.say("there is already another person on the apply list!")
         } else {
         newapply == user.name
         return this.say("you were added to the apply list.")
         }
         }
        }
       },
    
        checkapplylist: function (target, room, user) {
        if (!room instanceof Users.User) return;
        if (!user.isDeveloper()) {
        return this.say("You do not have permission to use this command!")    
        } else {
        return this.say("Apply list: " + newapply);    
         }
        },
    
        clearapplylist: function (target, room, user) {
        if (!room instanceof Users.User) return;
        if (user.isDeveloper()) {
        newapply = "none"
        return this.say("Cleared the apply list.")   
         } else {
        return this.say("You do not have permission to use this command!")  
         }
        },
    
        sobre: function (target, room, user) {
		if (room instanceof Users.User) {
		return this.say("Hola, yo soy " + Config.username + "! Soy un bot. Mi carácter de comando es ``. ``. Para obtener ayuda, pm usuario pichuhat. Estoy basado en el sistema de robots de sirDonovan, Cassius. puedes verlo aqui: https://github.com/sirDonovan/Cassius");
        } else {
        console.log("isdeveloper", user.isDeveloper())
        if (!user.isDeveloper() && !user.hasRank(room, "+")) return;
        return this.say("Hola, yo soy " + Config.username + "! Soy un bot. Mi carácter de comando es ``. ``. Para obtener ayuda, pm usuario pichuhat. Estoy basado en el sistema de robots de sirDonovan, Cassius. puedes verlo aqui: https://github.com/sirDonovan/Cassius")  
       }
    },
    
	ayuda: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper() && !user.hasRank(room, "+")) return;
        console.log("we made it!")
		return this.say("la guía de susbot está en inglés. ¡perdón!");
	},
    
    	empezaruno: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper() && !user.hasRank(room, "+")) return;
        console.log("we made it!")
        if (!room.id == "botdevelopment") {
        return this.say("no puedo iniciar un juego de uno en " + room.id)  
        } else {
        this.say ("/uno create 10")
        return this.say("/wall un juego de uno fue iniciado por " + user.name)
         }
        },
    
    	terminaruno: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper() && !user.hasRank(room, "+")) return;
        console.log("we made it!")
		this.say("/uno end")
        return this.say("/wall el juego de uno fue terminado a la fuerza por " + user.name + ".");
        },
    
    	continuaruno: function (target, room, user) {
		if (room instanceof Users.User) return;
        console.log("isdeveloper", user.isDeveloper());
        if (!user.isDeveloper() && !user.hasRank(room, "+")) return;
        console.log("we made it!")
		this.say("/uno start")
        return this.say("/wall ¡El juego de uno está comenzando!");
        },
    
        anfitriónaep: function (target, room, user) {
		if (room instanceof Users.User) return;
        if (hostable == 1) {
        hoster = user.name
		return this.say("/adduhtml susbothelp, <h1>" + user.name + " está organizando un juego de Adivina el Pokemon!</h1> <small>" + user.name + "</small>");
         } else {
             return this.say("/sendprivatehtmlbox " + user.name + ", <p>el alojamiento está actualmente deshabilitado. ¡" + user.name + ", inténtelo de nuevo más tarde!</p>");
         }
        },
    
        finanfitriónaep: function (target, room, user) {
		if (room instanceof Users.User) return;
        if (!hoster == user.name) return;
        hoster = "none"
		return this.say("/adduhtml susbothelp, <h1>¡La hueste de" + user.name + " de adivina que el pokemon había terminado!</h1> <small>" + user.name + "</small>");
        },
    
        selfclear: function (target, room, user) {
		if (room instanceof Users.User) return;
		return this.say("/hidetext " + user.name + ", este usuario borró intencionalmente sus propios mensajes");
        },
   
        permitiranfitrion: function (target, room, user) {
          if (room instanceof Users.User) return;
          console.log("isdeveloper", user.isDeveloper())
          console.log("hasrank", user.hasRank(room, "@"))
          if (!user.isDeveloper() && user.hasRank(room, "@")) return;
          hostable = 1
          return this.say("El alojamiento de usuarios ahora está permitido hasta que se reciba ``.deshabilitarhost``.");
        },
    
        deshabilitarhost: function (target, room, user) {
          if (room instanceof Users.User) return;
          console.log("isdeveloper", user.isDeveloper())
          if (!user.isDeveloper() && !user.hasRank(room, "@")) return;
          hostable = 0
          return this.say("El alojamiento de usuarios ahora está deshabilitado hasta que se reciba ``.permitiranfitrión``");
        },
	// Game commands
	signups: 'creategame',
	creategame: function (target, room, user) {
		if (room instanceof Users.User) return;
		if (!user.hasRank(room, '+')) return;
		if (!Config.games || !Config.games.includes(room.id)) return this.say("Games are not enabled for this room.");
		let format = Games.getFormat(target);
		if (!format || format.inheritOnly) return this.say("The game '" + target + "' was not found.");
		if (format.internal) return this.say(format.name + " cannot be started manually.");
		Games.createGame(format, room);
		if (!room.game) return;
		room.game.signups();
	},
	start: 'startgame',
	startgame: function (target, room, user) {
		if (!(room instanceof Users.User) && !user.hasRank(room, '!')) return;
		if (room.game) room.game.start();
	},
	cap: 'capgame',
	capgame: function (target, room, user) {
		if (room instanceof Users.User || !room.game || !user.hasRank(room, '!')) return;
		let cap = parseInt(target);
		if (isNaN(cap)) return this.say("Please enter a valid player cap.");
		if (cap < room.game.minPlayers) return this.say(room.game.name + " must have at least " + room.game.minPlayers + " players.");
		if (room.game.maxPlayers && cap > room.game.maxPlayers) return this.say(room.game.name + " cannot have more than " + room.game.maxPlayers + " players.");
		room.game.playerCap = cap;
		this.say("The game will automatically start at **" + cap + "** players!");
	},
	end: 'endgame',
	endgame: function (target, room, user) {
		if (!(room instanceof Users.User) && !user.hasRank(room, '!')) return;
		if (room.game) room.game.forceEnd();
	},
	join: 'joingame',
	joingame: function (target, room, user) {
		if (room instanceof Users.User || !room.game) return;
		room.game.join(user);
	},
	leave: 'leavegame',
	leavegame: function (target, room, user) {
		if (room instanceof Users.User || !room.game) return;
		room.game.leave(user);
	},
	// Storage commands
    //none!
	// Tournament commands
	tour: 'tournament',
	tournament: function (target, room, user) {
		if (room instanceof Users.User || !Config.tournaments || !Config.tournaments.includes(room.id)) return;
		if (!target) {
			if (!user.hasRank(room, '+')) return;
			if (!room.tour) return this.say("I am not currently tracking a tournament in this room.");
			let info = "``" + room.tour.name + " tournament info``";
			if (room.tour.startTime) {
				return this.say(info + ": **Time**: " + Tools.toDurationString(Date.now() - room.tour.startTime) + " | **Remaining players**: " + room.tour.getRemainingPlayerCount() + '/' + room.tour.totalPlayers);
			} else if (room.tour.started) {
				return this.say(info + ": **Remaining players**: " + room.tour.getRemainingPlayerCount() + '/' + room.tour.totalPlayers);
			} else {
				return this.say(info + ": " + room.tour.playerCount + " player" + (room.tour.playerCount > 1 ? "s" : ""));
			}
		} else {
			if (!user.hasRank(room, '%')) return;
			let targets = target.split(',');
			let cmd = Tools.toId(targets[0]);
			let format;
			switch (cmd) {
			case 'end':
				this.say("/tour end");
				break;
			case 'start':
				this.say("/tour start");
				break;
			default:
				format = Tools.getFormat(cmd);
				if (!format) return this.say('**Error:** invalid format.');
				if (!format.playable) return this.say(format.name + " cannot be played, please choose another format.");
				let cap;
				if (targets[1]) {
					cap = parseInt(Tools.toId(targets[1]));
					if (cap < 2 || cap > Tournaments.maxCap || isNaN(cap)) return this.say("**Error:** invalid participant cap.");
				}
				this.say("/tour new " + format.id + ", elimination, " + (cap ? cap + ", " : "") + (targets.length > 2 ? ", " + targets.slice(2).join(", ") : ""));
			}
		}
	},
	settour: 'settournament',
	settournament: function (target, room, user) {
		if (room instanceof Users.User || !Config.tournaments || !Config.tournaments.includes(room.id) || !user.hasRank(room, '%')) return;
		if (room.id in Tournaments.tournamentTimers) {
			let warned = overwriteWarnings.has(room.id) && overwriteWarnings.get(room.id) === user.id;
			if (!warned) {
				overwriteWarnings.set(room.id, user.id);
				return this.say("A tournament has already been scheduled in this room. To overwrite it, please reuse this command.");
			}
			overwriteWarnings.delete(room.id);
		}
		let targets = target.split(',');
		if (targets.length < 2) return this.say(Config.commandCharacter + "settour - tier, time, cap (optional)");
		let format = Tools.getFormat(targets[0]);
		if (!format) return this.say('**Error:** invalid format.');
		if (!format.playable) return this.say(format.name + " cannot be played, please choose another format.");
		let date = new Date();
		let currentTime = (date.getHours() * 60 * 60 * 1000) + (date.getMinutes() * (60 * 1000)) + (date.getSeconds() * 1000) + date.getMilliseconds();
		let targetTime = 0;
		if (targets[1].includes(':')) {
			let parts = targets[1].split(':');
			let hours = parseInt(parts[0]);
			let minutes = parseInt(parts[1]);
			if (isNaN(hours) || isNaN(minutes)) return this.say("Please enter a valid time.");
			targetTime = (hours * 60 * 60 * 1000) + (minutes * (60 * 1000));
		} else {
			let hours = parseFloat(targets[1]);
			if (isNaN(hours)) return this.say("Please enter a valid time.");
			targetTime = currentTime + (hours * 60 * 60 * 1000);
		}
		let timer = targetTime - currentTime;
		if (timer <= 0) timer += 24 * 60 * 60 * 1000;
		Tournaments.setTournamentTimer(room, timer, format.id, targets[2] ? parseInt(targets[2]) : 0);
		this.say("The " + format.name + " tournament is scheduled for " + Tools.toDurationString(timer) + ".");
	},
	canceltour: 'canceltournament',
	canceltournament: function (target, room, user) {
		if (room instanceof Users.User || !Config.tournaments || !Config.tournaments.includes(room.id) || !user.hasRank(room, '%')) return;
		if (!(room.id in Tournaments.tournamentTimers)) return this.say("There is no tournament scheduled for this room.");
		clearTimeout(Tournaments.tournamentTimers[room.id]);
		this.say("The scheduled tournament was canceled.");
	},
};

module.exports = commands;
