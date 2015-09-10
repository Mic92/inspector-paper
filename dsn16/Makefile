TARGET   = main
TEXFILES = $(wildcard *.tex)
FIGURES  = 
LATEX = latex

UNAME_S := $(shell uname -s)
ifeq ($(UNAME_S),Darwin)
    OPEN = open
else
    OPEN = xdg-open
endif

.SUFFIXES:	.plot .eps .fig

all: $(TARGET).pdf

figures/%.eps: plots/%.plot
	gnuplot $< > $@

graphs: $(addprefix figures/, $(addsuffix .eps, $(FIGURES)))

$(TARGET).aux: $(TEXFILES)
	$(LATEX) $(TARGET).tex

$(TARGET).bbl: $(TARGET).aux $(TARGET).bib
	bibtex $(TARGET)

$(TARGET).dvi: $(TEXFILES)   $(addprefix figures/, $(addsuffix .eps, $(FIGURES))) $(TARGET).bbl
	$(LATEX) $(TARGET).tex
	$(LATEX) $(TARGET).tex

$(TARGET).ps: $(TARGET).dvi
	dvips $< -Ppdf -Pcmz -Pamz -t letter -D 600 -G0 -o $@

$(TARGET).pdf: $(TEXFILES)   $(addprefix figures/, $(addsuffix .eps, $(FIGURES))) $(TARGET).bbl
	latexmk -f -pdf main.tex

view:: $(TARGET).pdf
	$(OPEN) $(TARGET).pdf

see:: $(TARGET).dvi
	xdvi $(TARGET)

%.eps: plots/%.plot
	gnuplot $< > figures/$@

%.eps: figures/%.fig
	fig2dev -L eps -p dummy $< $*.eps

spell::
	ispell *.tex

clean::
	rm -fv *.out *.dvi *.aux *.log *~ *.bbl *.blg *.toc *.ps *.pdf *.ent *.fls *.fdb_latexmk
	rm -fv $(addprefix figures/, $(addsuffix .eps, $(FIGURES)))

distclean:: clean
	rm $(TARGET).ps
