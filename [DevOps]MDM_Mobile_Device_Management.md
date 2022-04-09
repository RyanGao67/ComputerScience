### active directory, domains, remote access, virtualization, cloud stuff 

You go back to the 1950s, the 1960s that had mainframes, these massive computers that would take up the size of a room.

And then eventually they became sort of the size of a refrigerator. And then as we got to the 1970s, people started making their own computers.

And we got to the the late 70s and the concept of personal computing came out. And then by the time we got to the 80s, you actually started seeing computers in businesses.

And so you're companies out there would have actual computers that your office staff could use.

let's say you've got a thousand computers in your business. And of course, in the early days of computers, we lived in what was called a peer to peer network. Peer to peer network basically means that every computer is equal there.There is no real authority over each individual computer.In other words, if your boss walks up to you and says, Hey it person, I want you to configure fivethings on all 1000 computers, you had to either A sit down at all 1000 computers and configure them one at a time, or B you had to write a script to do it.So peer to peer networking was is not a great way to try to achieve things centrally.

So what happened was a company called Novell, they created a product called NetWare.They really changed things.They really pushed this whole concept of what is called client servers, where you have a more powerful computer, maybe not the, the the size of a mainframe, but a more powerful computer. And your client computers interact with that server

Well, then, of course, Microsoft eventually got into the networking world as well.They created this thing called the Domain, which was very similar to what Novell was doing.

And then you reached the year 2000 where things really sort of change.Microsoft comes out with a new type of domain.They start using the triangle as their symbol of a domain.

And one of the main concepts of a domain is that you have a special type of server and the server is called a domain controller.

So we'll just put D.C..And inside that domain controller, you have a database. I'm just going to draw this little cylinder looking thing for you guys here.This is your your database now, your database is called the Active Directory database, all right.And so your domain controller is what controls your domain.

So if you really want to know, like, why are domains important?All right.My answer to you is one word, and that is centralization.A lot of people think security security's important, definitely.But centralization is why domains are so important, because they allow us to control all of our stuff,our clients, our servers, all of that stuff centrally.We can manage it all in one place instead of having to sit down at each individual machine and makechanges.

OK, now, of course, you're going to want to have usually more than one domain controller.And the reason why you don't want to have more than one domain or is the same reason you want to havemore than one of anything really more than one of any server especially.And that is fault tolerance and load balancing 

OK, so these domain controllers also have these things called GPOS(group policy objects) which help us control everything.

So instead of having to sit down and one at a time and make changes to a machine, I can create this thing called the GPO. The GPO can deploy the settings out to the machines.

OK, now something else that's interesting about Active Directory, Active Directory being the directory services database and all that, that that you use, it uses a protocol called LDAP lightweight directory access protocol, which is basically like the language that your directory serves as speak user accounts, groups, all that stuff. Passwords are all kept in there. And it also uses a protocol called Kerberos for its security.

Now, domain controllers replicate. So whatever you do to one active directory database, it actually does it to the other.For example, if I create a little user account, will say that this little smiley face guy that I'm making here, he is going to represent a user account. All right. If we make this user account, we create the user account on the first domain controller here that'sactually going to replicate to the other domain controller and any of the other domain controllers thatare in the ACT directory domain.

So what I'm basically saying here is that these domain controllers replicate with each other.So when you when you change one database, it's going to replicate over to the other.So they stay in sync.

OK, now another important component of a Microsoft domain is your domain. Must have a name, OK? And the name that it uses is a DNS name, domain name system, also known as domain name space, also known as domain name service.OK, and this is because we human beings don't really like to identify devices by numbers.We don't really like having to memorize lots of numbers. We prefer to use names when we identify things. So DNS is the service that does that.OK, all your computers and services and all that, they have IP addresses that they use, which are numbers, but we like to associate things with words and names.So your domain will have a name like for example, if my domain is called exam lab practice dotcom, that's my company name.

A lot of times your domain name will be the same as your Web presence.

OK, so the other problem with that, though, is if you're going to you know, if your domain is going to have the name, you've got to have a DNS service that's going to manage all that.

So we actually have to have a server called the DNS server.Now, granted, your domain controller can actually play this role, but I'm going to draw it separatelyhere and inside that DNA database.

Inside that DNS server, we have what is called a DNS database.draw another one of these little cylinder looking things here and. The other thing that's interesting about that is that the database will be named after your domain.

So if your domain name is examlabpractice.com, your database is going to be called examlabpractice.com. That database is called a zone database, also known as a name database or namespace database. And what will happen is all of your computers, as they come online, they will automatically register their names inside that database, their names and their IP addresses.

So now when the computers want to find each other, they can actually query that DNS server to find each other.

So when these computers boot up, they have to authenticate to the to one of these domain controllers using Kerberos. They're going to do LDAP based queries to do that, but they're going to first actually query DNS andsay, hey, DNS, do you know who my domain controller is? And since those domain controllers have actually registered DNS, he'll reply back and then DNA and then the client can go and get authenticated. OK, and you you are officially authenticated. And then when a client wants to communicate with the file server, they'll actually query the for the file server through DNS as well. OK, so that is your basic back and forth that is happening to make all of this work.

Now we also have an Internet connection to think about. So let's draw this little cloud symbol here and we'll just kind of clean it up here.So our little cloud here is going to represent the Internet coming in.
And let me just label that Internet. Of course, you don't want your internal network to be, you know, completely unprotected, so you probably want to have a firewall, right?

Firewall, router type combination. So we'll say F W for firewall, put a little box around it.

That's going to be our firewall and that's going to help police traffic coming going out to the Internet and things coming in from the Internet.

And we'll talk more about, you know, things coming in here in a minute in the next foundation video.

But hopefully this gives you now a good understanding of just the basics of, you know, what is the domain? Why is it important?

It's centralization purposes. You can deploy group policy objects, which there are restrictions out there on people's machines and all that.




So as we move on here, I mentioned active directory. You know, it came out in the year 2000.Originally, it was called it was an operating system called enty five. And then Windows 2000 is when they released it with and they changed the name.They changed the name from 95 to Windows 2000.And the directory service used to be called NTD and they renamed it to Active Directory.OK, so 20 years ago, you know, Active Directory came out.This was cutting edge.It was great.It was awesome.

And one of the things that we sometimes need to happen is we need computers on the outside world toget to the inside world. So imagine you've got a computer here, maybe got somebody working from home and they need to be ableto access a resource, like maybe they need to access that file server.

Well, let me tell you what you don't want to do. You don't want to just open up a bunch of ports on your firewall and let things in like file serversand windows use a use a protocol called SMB Server Message Block, and that uses a port for four orfive.And there's a few other ports here, too.But you would be opening ports coming in to let things come in from the Internet.And that's very dangerous, right?We don't want to do that on our firewall.We don't want to open up our ports and all that because we're just asking for a bad guy.All right.We're asking for Hacker to attempt to get into our environment.Right.So let me just make a hacker here.All right, let's zoom in on here, and we're going to make him a bad guy here.Let's give him some devil horns.Let's give him a devil tail.All right.We'll make him look like he's just in a bad mood.All right.


And we don't want we don't want to open up a bunch more because we don't want a hacker to be able toget in.
Now, we do have somebody over here, though, that we do want to get in.So maybe we're going to let them do that.Now, there's a couple of ways you can do that.

One would be to get something called a VPN concentrator or VPN router to basically an appliance, a

box that you put on your network and you let people securely get in.

Now, the Microsoft way sort of their solution usually is to set up a server called a RAS server,sometimes referred to as RRASserver, OK?And that server is a remote access server.And from what we can do there is we can allow access with our firewall.We can allow what's known as a VPN, a virtual private network.So from there, your you can securely connect from the outside world to that server and you can accessthis file server or some other resources.And it's all going to be encrypted using what's called an encrypted tunnel.And that's going to be one of your main ways and one of your safest ways to allow things to come in.Another thing, Microsoft Sports, this thing called direct access, which are not really focusing alot on that these days, but VPN is going to be one of the main ways that you do this.OK, there are other solutions.You can set up this thing called a remote desktop gateway and allow remote desktop in.But VPN is going to be your main core way to do things, OK?

Now, the other thing I'd like to mention is what about situations where we have something like a server

that we need to make available to the outside world?

OK, let me give you an example.

Let's say that you are hosting your own Web server. You're going to host your own Web server.


Well, I'm going to tell you, if you hosted in here, that's dangerous because, you know, if you let people get to your Web server anonymously, then, you know, you've got people coming in here and and going through your firewall and accessing that Web server. Well, you know, hackers can do this thing called pivoting.If they gain control over that server, they might be able to pivot to other resources.So that would not be a good idea. A better solution would be to store the Web server out here, OK.

The problem, though, with storing it outside your firewall like that is you are not really givingyour Web server any protection.

It's exposed completely to the elements at that point.

It's outside the firewall.

So generally, the rule of thumb, what most people would do with this is they would get. They would actually get a another firewall, OK, and and they put the other firewall actually next to their Internet, and at that point you have something called a DMZ, a demilitarized zone, also known as a perimeter network.

So there are a couple of names for that DMZ perimeter network.

And the great thing about that is I can police now the traffic flowing in through the firewall.

I can police that I might allow, like, for example, port eighty four, four, three.

You might have a DNS and if you do, you do a port 53.But now people can get to this web server.Coming in, but then you could block everything except maybe your VPN and all that going through thatfirewall, so that is the idea of a DMZ.So if a hacker was to gain control over that Web server, they would still be blocked and would notbe able to flow through that firewall.That's the logic of this.

All right.

So now the so the concept here is there's multiple solutions when it comes to trying to allow peopleout, allow people in.If you're wanting to host something like a Web server, maybe you want to do the same thing with anemail server.

Microsoft has exchange and you have a type of exchange roll called edX, which, of course, we could have a what's called an exchange server in there.

And really, you know, the mentality there is the logic has always been with with the world that at

least in I.T., we've always kind of felt as it people we needed to sort of host everything ourselves

and manage everything ourselves.

And I want to I want to kind of go over a couple of things in that regard.


So first off, I talked about centralization in a domain and the fact that we have to have central control,

those are going to be one of our main ways to do that.

Microsoft also has another server that helps you get even more control over your environment.

And in the 90s, they called it SMS System Management Services.

And then in the early 2000s, they renamed this server that I'm about to tell you about to Eskom, which

is a system center configuration manager, OK?

And so for the longest time, that's what it is.

That's what it was called.

And system center configuration manager can even more so control your devices.

It can inventory devices, you can deploy images, you can deploy applications, you can configure compliance.

I mean, there's a ton of stuff you can do with them.

All right.

And more recently, that got renamed to another product.

It is no longer called Eskom.

Hopefully this is not a shocker to you.

It is now called Endpoint Config Manager.

I'm just going to put Seyoum endpoint configuration manager.

All right.

And this is because of a product that they're deploying in the cloud called into that has sort of become

the big cloud based system for managing things.

And what they want is they want in they want config manager and this other product intune to sort of

centrally work together.

And so they renamed config manager to endpoint configuration manager.

And so that's what it is now referred to as.

All right.

OK, now, the last thing I want to mention here is that another sort of logic that we've had over the

years when it comes to managing things as it people, is that, you know, we got to have all these

different servers.

Right?

Like, we have to have an exchange server.

All right.

We got to have a SharePoint server all to say S.P..

We got to have.

A database server like sequel as Keywell and we've already got a file server.

Right.

Let me use that little file server here.

Actually, alwa just for the sake of keeping the font size of the same will, we'll just make it a little

bit smaller so it'll fit here.

So we got a file server.

Now, the mentality has always been that.

We have to host all these servers ourselves, OK, and we have to have all this equipment in order to

do it.

And so you end up with a lot of servers in order to to deal with everything.

OK, so in the early 2000s, a company called VMware really came came to the to the table with some

some innovative solutions for dealing with the problem of having so many servers, so much hardware.

And they came up with I actually used an old solution to fix newer problems.

There's a thing called hypervisor.

This is a term that actually came out in the 1970s, back in the Unix days, early Unix days.

And it was a virtualization idea.

Hyper hypervisor is not a new thing.

It's actually been around for a long time.

OK, so they actually really came up with some cutting edge stuff on this.

And the concept was that, you know, if you can emulate hardware, you know, processors and RAM and

storage and network, you can then put software on that insulated hardware.

So the idea of a virtual machine, you know, is a simulated hardware and you've got software installed

on that like like server, Windows 10, Windows Server, all that.

So you could take these four servers, these four physical servers, and you could actually run them

on a single physical server.

OK, so instead of having so many different, you know, pieces of hardware here that you're trying

to deal with and manage, you could do it all on a physical server.

Of course, a lot of people, when they see that, they're like, I mean, now you've got a single point

of failure.

Well, I'll get to that in a second.

So VMware really took off.

Microsoft definitely took note of what they did.

Microsoft actually bought a product called Virtual PC and then turned it into a virtual server and then

eventually they renamed it to Hyper V.

So that is actually the Microsoft virtualization solution, even though VMware still probably considered

more popular than than hypervisor.

So the other thing that we've got here is the fact that we have a single point of failure.

I mentioned if that server dies, we're in trouble.

Right?

We'll see.

That's the beauty of virtualization.

We have the ability of actually very easily getting another server.

All right.

Let me strength this down a little bit.

We can get another server.

We can use a storage area network, we can host our virtual machines on that, and the hyper V servers

can do clustering so you can get a very high level of redundancy with the help of virtualization.

OK, and so that's the idea of virtualization.

Now, with virtualization came another term and it's the term elasticity justis.

That means that those servers can have tons of memory, tons of storage, tons of network bandwidth,

tons of CPU usage.

And they can pool it, so if the exchange virtual machine needs more memory and the sequel virtual machine

doesn't, then, you know, school can give up the memory.

It's not using an exchange.

Virtual machine can use that memory and CPU usage and so it can shrink and grow in strength and grow.

OK, and that's one of the greatest benefits of virtual machines, another great benefit of virtual

machines is the fact that you have these things called checkpoints, all of which used to be called

snapshots.

OK, but they're now called checkpoints, at least in hyper v terms.

They are.

So this really, really changed things.

And as you'll see, this is also sort of the forerunner of cloud computing.

All right.

OK, but hopefully that gives you guys now a decent foundation of some of the other concepts, Remote

Access DMZ, as well as the basic idea of virtualization.
