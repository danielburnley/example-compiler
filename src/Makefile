PACKAGES = str

FILES = util tokens sourceAst typeCheck frontEnd constProp unnestExp \
blockStructure shrinkImmediates liveVarAnalysis regAlloc lineariseCfg x86 \
instrSelX86 compileFunction astInterp blockInterp interp compile

FILES_ML = $(addsuffix .ml,$(FILES))
FILES_MLI = $(addsuffix .mli,$(FILES))
FILES_CMI = $(addsuffix .cmi,$(FILES))
FILES_CMT = $(addsuffix .cmt,$(FILES))
FILES_CMTI = $(addsuffix .cmti,$(FILES))
FILES_CMO = $(addsuffix .cmo,$(FILES))
FILES_CMX = $(addsuffix .cmx,$(FILES))
FILES_O = $(addsuffix .o,$(FILES))

.PHONY : ocb-compile.byte ocb-compile.native ocb-interp.byte ocb-interp.native all

all : ocb-interp.byte ocb-compile.byte

ocb-interp.byte :
	rebuild -use-ocamlfind interp.byte

ocb-interp.native :
	rebuild -use-ocamlfind interp.native

interp.byte : $(FILES_CMO)
	ocamlfind ocamlc -linkpkg -package $(PACKAGES) $(FILES_CMO) -o $@

interp.native : $(FILES_CMX)
	ocamlfind ocamlopt -linkpkg -package $(PACKAGES) $(FILES_CMX) -o $@

ocb-compile.byte :
	rebuild -use-ocamlfind compile.byte

ocb-compile.native :
	rebuild -use-ocamlfind compile.native

compile.byte : $(FILES_CMO)
	ocamlfind ocamlc -linkpkg -package $(PACKAGES) $(FILES_CMO) -o $@

compile.native : $(FILES_CMX)
	ocamlfind ocamlopt -linkpkg -package $(PACKAGES) $(FILES_CMX) -o $@

%.cmi : %.mli
	ocamlfind ocamlc -package $(PACKAGES) -bin-annot -o $@ -c $<

%.cmo : %.ml
	ocamlfind ocamlc -package $(PACKAGES) -bin-annot -o $@ -c $<

%.cmx : %.ml
	ocamlfind ocamlopt -package $(PACKAGES) -bin-annot -o $@ -c $<

.PHONY : depend clean

clean :
	rebuild -clean
	rm -f $(FILES_CMI) $(FILES_CMTI) $(FILES_CMT) $(FILES_CMO) $(FILES_CMX) $(FILES_O) compile.byte compile.native interp.byte interp.native

depend :
	ocamlfind ocamldep $(FILES_ML) $(FILES_MLI) > .depend

include .depend
