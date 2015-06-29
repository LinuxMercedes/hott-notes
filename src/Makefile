SOURCEFILES=$(wildcard *.md)
OUTPUTFILES=$(SOURCEFILES:.md=.html)

all: $(OUTPUTFILES)

%.html : %.md
	pandoc --filter=./tikz-filter.py --standalone -c style.css -B header.html $< -o ../$@
