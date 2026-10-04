# The page where the response letter starts is recorded in the .aux file by
# responsehelper.sty (\zlabel{responsehelper:start}, absolute page number),
# so the PDF is split at the right page without being modified or parsed.
#
# latexwrapper builds inside ./.<name>.build/ and only copies the PDF back,
# so the .aux file is looked up there first, then in the current directory.

AUX_DIRS = .paper.build .

all:
	mkdir -p ./out
	cp ../responsehelper.sty ./
	latexwrapper paper.tex
	@AUX=; for d in $(AUX_DIRS); do [ -f "$$d/paper.aux" ] && AUX="$$d/paper.aux" && break; done; \
	START=; [ -n "$$AUX" ] && START=$$(sed -n 's/.*zref@newlabel{responsehelper:start}.*\\abspage{\([0-9]*\)}.*/\1/p' "$$AUX" | tail -n 1); \
	if [ -z "$$START" ] || [ "$$START" -lt 2 ]; then \
	  echo "error: could not find the response start page in paper.aux (searched: $(AUX_DIRS))" >&2; \
	  echo "       is \\begin{responseletter} used, and did the build succeed?" >&2; exit 1; \
	fi; \
	echo "response starts at page $$START (from $$AUX)"; \
	pdftk paper.pdf cat 1-$$((START-1)) output ./out/2_paper_marked.pdf && \
	pdftk paper.pdf cat $$START-end     output ./out/3_response.pdf
	mv paper.pdf ./out/1_paper_and_response.pdf
	sed 's/\\usepackage{responsehelper}/\\usepackage[clean]{responsehelper}/' paper.tex > clean.tex
	latexwrapper clean.tex
	mv clean.pdf ./out/4_paper.pdf
	latexwrapper c paper.tex
	latexwrapper c clean.tex
	rm clean.tex responsehelper.sty
