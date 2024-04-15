all:
	mkdir -p ./out
	cp ../responsehelper.sty ./
	latexwrapper paper.tex
	pdftk paper.pdf cat 1-1   output ./out/2_paper_marked.pdf
	pdftk paper.pdf cat 2-end output ./out/3_response.pdf
	mv paper.pdf ./out/1_paper_and_response.pdf
	sed 's/\\usepackage{responsehelper}/\\usepackage[clean]{responsehelper}/' paper.tex > clean.tex
	latexwrapper clean.tex
	mv clean.pdf ./out/4_paper.pdf
	latexwrapper c paper.tex
	latexwrapper c clean.tex
	rm clean.tex responsehelper.sty
