NAME=engine-dwh-reports-on-separate-hosts

all: plain presentation self-contained zip

presentation: $(NAME).md
	pandoc -B ovirt-background.hpart -t slidy -c ovirt-slidy.css -s -o $(NAME)-presentation.html $(NAME).md

self-contained: $(NAME).md
	pandoc --self-contained -B ovirt-background.hpart -t slidy -c ovirt-slidy.css -s -o $(NAME)-presentation-self-contained.html $(NAME).md

plain: $(NAME).md
	pandoc -t html --self-contained -o $(NAME).html $(NAME).md

zip: presentation self-contained
	zip -r $(NAME).zip ovirt* slidy $(NAME)*.html $(NAME).md

clean:
	/bin/rm -f *.html *.zip

