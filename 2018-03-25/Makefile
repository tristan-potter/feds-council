LATEX:=pdflatex
LATEXOPT:=--shell-escape
NONSTOP:=--interaction=nonstopmode

LATEXMK:=latexmk
LATEXMKOPT:=-pdf
CONTINUOUS:=-pvc

CONFIDENTIAL:=confidential.tex
PUBLIC:=public.tex

mkfile_path:=$(abspath $(lastword $(MAKEFILE_LIST)))

MAIN:=$(notdir $(patsubst %/,%,$(dir $(mkfile_path))))
CONFIDENTIAL_OUT:=$(MAIN)-confidential
PUBLIC_OUT:=$(MAIN)-public
COMPILATION_FOLDER:=bin
DIST_FOLDER:=dist

SOURCES:=$(DIST_FOLDER) $(COMPILATION_FOLDER)
SOURCES_C:=$(SOURCES) $(CONFIDENTIAL) Makefile
SOURCES_P:=$(SOURCES) $(PUBLIC) Makefile

all: confidential public

.refresh:
	touch .refresh

$(DIST_FOLDER):
	mkdir ./dist

$(COMPILATION_FOLDER):
	mkdir ./bin

watch: .refresh $(SOURCES_C) 
	$(LATEXMK) $(LATEXMKOPT) $(CONTINUOUS) \
		-pdflatex="$(LATEX) $(LATEXOPT) $(NONSTOP) %O %S" $(CONFIDENTIAL) \
		-jobname=$(COMPILATION_FOLDER)/$(CONFIDENTIAL_OUT)

confidential: $(SOURCES_C)
	$(LATEXMK) $(LATEXMKOPT) \
		-pdflatex="$(LATEX) $(LATEXOPT) $(NONSTOP) %O %S" $(CONFIDENTIAL) \
		-jobname=$(COMPILATION_FOLDER)/$(CONFIDENTIAL_OUT)
	mv ./$(COMPILATION_FOLDER)/$(CONFIDENTIAL_OUT).pdf ./$(DIST_FOLDER)/$(CONFIDENTIAL_OUT).pdf

public: $(SOURCES_P)
	$(LATEXMK) $(LATEXMKOPT) \
		-pdflatex="$(LATEX) $(LATEXOPT) $(NONSTOP) %O %S" $(PUBLIC) \
		-jobname=$(COMPILATION_FOLDER)/$(PUBLIC_OUT)
	mv ./$(COMPILATION_FOLDER)/$(PUBLIC_OUT).pdf ./$(DIST_FOLDER)/$(PUBLIC_OUT).pdf

clean:
	rm -rf *~ *.tmp
	rm -f *.bbl *.blg *.aux *.end *.fls *.log *.out *.fdb_latexmk
	rm -rf $(COMPILATION_FOLDER)

debug:
	$(LATEX) $(LATEXOPT) $(MAIN)

.PHONY: clean force public confidential watch
