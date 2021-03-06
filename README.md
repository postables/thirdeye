Ridiculously Easy i2p Jump Service
==================================

This is an i2p Address Helper/Jump service implementation, basically cloned
from the already available one from
[robertfoss/i2pjump](https://github.com/robertfoss/i2pjump) in Go, which some
people prefer, with a few minor differences from the original. For no particular
reason right now, I made it use the SAM bridge to retrieve remote addressbooks,
thereby creating a new destination for every time an addressbook is retrieved
and preventing the upstream addressbook provider from being able to associate a
single long-term identity with retrieving addressbooks. I think this might
de-motivate a rubber-hose attacker who tried to get a jump service owner to
serve incorrect addresses by force, because the jump service operator would have
no way of deciding who to feed bad addresses to. But I mean, if there's a dude
ready to beat you with a hose, he's probably not uncomfortable with just serving
bad addresses to everybody and at any rate, you've probably got bigger problems.
It's just a hypothetical rationale. I've got some other idea for the future,
too, but I'll get to them later. It uses a regular i2p tunnel to provide it's
eepSite, so that does have a long-term destination, which it needs to be useful.
It also stores the list of hosts as a plain text file instead of a database, for
no particular reason but I think it's OK for the small number of hosts for now.

To deploy it, you can just:

    git clone https://github.com/eyedeekay/thirdeye
    cd thirdeye
    make update

Flags/Defaults:
------

        title       := flag.String("title",
            "Thirdeye Based Jump Service", "Title of the service.")
        desc        := flag.String("desc",
            "Thirdeye based jump service", "Brief description of the service.")
        logwl       := flag.String("logwl",
            "", "Whitelist of urls to never log")
        samhost     := flag.String("samhost",
            "127.0.0.1", "Host address to attach to SAM bridge.")
        samport     := flag.String("samport",
            "7656", "SAM port.")
        host        := flag.String("host",
            "0.0.0.0", "Host address to listen on.")
        port        := flag.String("port",
            "8053", "Port to listen on.")
        retries     := flag.Int("retries",
            2, "Number of attempts to fetch new hosts")
        interval    := flag.Int("interval",
            6, "Hours between updatess")
        newhosts    := flag.String("newhosts",
            "http://stats.i2p/cgi-bin/newhosts.txt", "Fetch new hosts from here")
        upstream    := flag.String("upstream",
            "http://i2p2.i2p/hosts.txt", "Fetch more hosts from here")
        hostfile    := flag.String("hostfile",
            "etc/thirdeye/localhosts.txt", "Local hosts file")
        cssfile     := flag.String("cssfile",
            "etc/thirdeye/style.css", "Local css file")
        icofile     := flag.String("icofile",
            "etc/thirdeye/favicon.ico", "Local favicon file")
        debug       := flag.Bool("debug",
            false, "Print connection debug info")
        verbosity   := flag.Int("verbosity",
            4, "Verbosity level: 0=Quiet 1=Fatal 2=Warning 3=Debug")

Launch and Add all Known Jump Services:
=======================================

        thirdeye --upstream http://no.i2p/export/alive-hosts.txt,http://i2pjump.i2p/hosts,http://i2p-projekt.i2p/hosts.txt,http://i2p2.i2p/hosts.txt

