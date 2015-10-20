TARGET   = main
TEXFILES = $(wildcard *.tex)
FIGURES  = 
PDFLATEX = pdflatex

.SUFFIXES:	.plot .eps .fig

all: $(TARGET).pdf

figures/%.eps: plots/%.plot
	gnuplot $< > $@

graphs: $(addprefix figures/, $(addsuffix .eps, $(FIGURES)))

$(TARGET).aux: $(TEXFILES)
	$(PDFLATEX) $(TARGET).tex

$(TARGET).bbl: $(TARGET).aux $(TARGET).bib
	bibtex $(TARGET)

$(TARGET).pdf: $(TEXFILES)   $(addprefix figures/, $(addsuffix .eps, $(FIGURES))) $(TARGET).bbl
	latexmk -pdf main.tex

view:: $(TARGET).pdf
	open $(TARGET).pdf

%.eps: plots/%.plot
	gnuplot $< > figures/$@

%.eps: figures/%.fig
	fig2dev -L eps -p dummy $< $*.eps

spell::
	ispell *.tex

clean::
	rm -fv *.out *.aux *.log *~ *.bbl *.blg *.toc *.ps *.pdf *.ent *.fls *.fdb_latexmk
	rm -fv $(addprefix figures/, $(addsuffix .eps, $(FIGURES)))

distclean:: clean
	rm $(TARGET).ps
