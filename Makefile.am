# Copyright 2017 Declan Hoare

shelltris:
	sed "s|/var|@localstatedir@|g" < $(srcdir)/shelltris.sh > shelltris
	chmod +x shelltris

install-data-local:
	install -d -m775 --group=games $(DESTDIR)/$(localstatedir)/games/shelltris
	install -m664 --group=games $(srcdir)/src/highscore.sh $(DESTDIR)/$(localstatedir)/games/shelltris/highscore.sh
	install -m664 --group=games $(srcdir)/src/calibrate.sh $(DESTDIR)/$(localstatedir)/games/shelltris/calibrate.sh
