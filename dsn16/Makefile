TARGET   = main
TEXFILES = $(wildcard *.tex)
FIGURES  =
GRAPHS   = arch arch2
LATEX = latex

UNAME_S := $(shell uname -s)
ifeq ($(UNAME_S),Darwin)
    OPEN = open
else
    OPEN = xdg-open
endif

.SUFFIXES:	.plot .eps .fig .dot .jpg

all: $(TARGET).pdf

figure/%.jpg: figure/%.dot
	dot -Tjpg $< -o$@

figure/%.eps: plots/%.plot
	gnuplot $< > $@

EPS_FIGURES = $(addprefix figure/, $(addsuffix .eps, $(FIGURES)))

DOT_FIGURES = $(addprefix figure/, $(addsuffix .jpg, $(GRAPHS)))

graphs: $(EPS_FIGURES) $(DOT_FIGURES)

$(TARGET).aux: $(TEXFILES)
	$(LATEX) $(TARGET).tex

$(TARGET).bbl: $(TARGET).aux $(TARGET).bib
	bibtex $(TARGET)

$(TARGET).pdf: $(TEXFILES) $(EPS_FIGURES) $(DOT_FIGURES) $(TARGET).bbl
	latexmk -f -pdf main.tex

view:: $(TARGET).pdf
	$(OPEN) $(TARGET).pdf

%.eps: plots/%.plot
	gnuplot $< > figure/$@

%.eps: figure/%.fig
	fig2dev -L eps -p dummy $< $*.eps

spell::
	ispell *.tex

clean::
	rm -fv *.out *.aux *.log *~ *.bbl *.blg *.toc *.ps *.pdf *.ent *.fls *.fdb_latexmk
	rm -fv $(EPS_FIGURES) $(DOT_FIGURES)

distclean:: clean
	rm $(TARGET).ps
