TEX = clowdr-paper

default: $(TEX).tex
	docker run -ti -v ${PWD}:${PWD} -w ${PWD} --entrypoint pdflatex gkiar/cv:compiled ${TEX}.tex
	docker run -ti -v ${PWD}:${PWD} -w ${PWD} --entrypoint bibtex gkiar/cv:compiled ${TEX}
	docker run -ti -v ${PWD}:${PWD} -w ${PWD} --entrypoint pdflatex gkiar/cv:compiled ${TEX}.tex
	docker run -ti -v ${PWD}:${PWD} -w ${PWD} --entrypoint pdflatex gkiar/cv:compiled ${TEX}.tex
	open $(TEX).pdf &

clean:
	$(RM) -f *.aux *.blg *.dvi *.log *.toc *.lof *.lot *.cb *.bbl $(TEX).ps *~ *.run.xml;

