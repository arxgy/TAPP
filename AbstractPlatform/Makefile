SMOKE?=0
ifeq ($(SMOKE),1)
	BOOGIESMOKE:=/smoke
else
	BOOGIESMOKE:=
endif
BOOGIE := boogie
BOOGIEOPT:=/proverOpt:O:smt.relevancy=0 /proverOpt:O:smt.case_split=0 /errorLimit:1 $(BOOGIESMOKE) \
					 /traceverify \
					 /trace	\
					 /printModel:1 \
					 /printModelToFile:log/model.log \
					 /errorTrace:2 \
					 /enhancedErrorMessages:0 \
					 /proverLog:log/prover.log \
					 /printCFG:dotfiles/ctrlflow \
					 /proc:ProveIntegrity \
					 /proverWarnings:1 \
					#  /timeLimit:900 \
					#  /break \
# /traceverify
# /errorTrace
COMMON=../Common
BUILD=build
CPU_TARGET:=$(BUILD)/CPU.xml
INTEGRITY_PROOF_TARGET=$(BUILD)/IntegrityProof.xml
MEM_CONFIDENTIALITY_PROOF_TARGET=$(BUILD)/MemConfidentialityProof.xml
CACHE_CONFIDENTIALITY_PROOF_TARGET=$(BUILD)/CacheConfidentialityProof.xml
PT_CONFIDENTIALITY_PROOF_TARGET=$(BUILD)/PTConfidentialityProof.xml
COMMON_PROOF_TARGET:=$(BUILD)/ProofCommon.xml
MEASUREMENT_TARGET:=$(BUILD)/Measure.xml
MEASUREMENT_PROOF_TARGET=$(BUILD)/MeasurementProof.xml
TEMP_PROOF_TARGET:=$(BUILD)/temp.xml

COMMON_FILES:=$(COMMON)/Types.bpl $(COMMON)/Common.bpl $(COMMON)/Cache.bpl
ATP_COMMON_FILES:=Types.bpl $(COMMON_FILES)
CPU_FILES:=CPU.bpl
CPU_IMPL_FILES:=CPUImpl.bpl
COMMON_PROOF_FILES:=ProofCommon.bpl
COMMON_IMPL_FILES:=ImplCommon.bpl
INTEGRITY_PROOF_FILES:=IntegrityProof.bpl $(COMMON_PROOF_FILES)
CONFIDENTIALITY_COMMON_FILES:=ConfidentialityProof.bpl $(COMMON_PROOF_FILES)
MEM_CONFIDENTIALITY_PROOF_FILES:=MemConfidentialityProof.bpl
CACHE_CONFIDENTIALITY_PROOF_FILES:=CacheConfidentialityProof.bpl
PT_CONFIDENTIALITY_PROOF_FILES:=PTConfidentialityProof.bpl
MEASUREMENT_FILES:=Measure.bpl 
MEASUREMENT_PROOF_FILES:=MeasurementProof.bpl
TEMP_PROOF_FILES:=temp.bpl

all: $(CPU_TARGET) $(MEASUREMENT_TARGET) $(COMMON_PROOF_TARGET) $(INTEGRITY_PROOF_TARGET) $(MEM_CONFIDENTIALITY_PROOF_TARGET) $(CACHE_CONFIDENTIALITY_PROOF_TARGET) $(PT_CONFIDENTIALITY_PROOF_TARGET) $(MEASUREMENT_PROOF_TARGET)
cpu: $(CPU_TARGET)
intiprf: $(INTEGRITY_PROOF_TARGET)
memconf: $(MEM_CONFIDENTIALITY_PROOF_TARGET)
measurepf: $(MEASUREMENT_PROOF_TARGET)
cmnpf : $(COMMON_PROOF_TARGET)

$(CPU_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(CPU_IMPL_FILES)
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(INTEGRITY_PROOF_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(INTEGRITY_PROOF_FILES)
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(MEM_CONFIDENTIALITY_PROOF_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(CONFIDENTIALITY_COMMON_FILES) $(MEM_CONFIDENTIALITY_PROOF_FILES)
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(CACHE_CONFIDENTIALITY_PROOF_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(CONFIDENTIALITY_COMMON_FILES) $(CACHE_CONFIDENTIALITY_PROOF_FILES)
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(PT_CONFIDENTIALITY_PROOF_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(CONFIDENTIALITY_COMMON_FILES) $(PT_CONFIDENTIALITY_PROOF_FILES)
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(COMMON_PROOF_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(COMMON_PROOF_FILES)  $(COMMON_IMPL_FILES)
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(MEASUREMENT_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(MEASUREMENT_FILES) $(COMMON_PROOF_FILES) 
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(MEASUREMENT_PROOF_TARGET): $(ATP_COMMON_FILES) $(CPU_FILES) $(MEASUREMENT_PROOF_FILES) $(MEASUREMENT_FILES) $(COMMON_PROOF_FILES) 
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^

$(TEMP_PROOF_TARGET):  $(TEMP_PROOF_FILES) $(COMMON_PROOF_FILES) $(CPU_FILES) $(ATP_COMMON_FILES)
	$(BOOGIE) $(BOOGIEOPT) /xml:$@ $^




clean:
	rm -f build/*.xml

cut:
	sed -n '/peep-hole/,$$p' log/run.log > log/peephole.log
	sed -n -e'/catch/,$$p' log/run.log > log/catch.log	
	sed '/passive/,$$d' log/catch.log > log/onlyCatch.log
	sed -n -e'/z3/,$$p' log/peephole.log > log/result.log	
	sed -i 's/#AT#/@/g' log/run.log 
	sed -i 's/#AT#/@/g' log/peephole.log
	rm log/catch.log


.PHONY: clean, cut
