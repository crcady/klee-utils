klee-patch.o: ../src/main.c
	g++ -c -fno-rtti -o klee-patch.o ../src/main.c -I /tmp/klee-build/include/ -I /tmp/klee/include/ -I /tmp/llvm-install/include/

Statistics.cpp.o: ../src/Statistics.cpp
	g++ -c -fno-rtti -o Statistics.cpp.o ../src/Statistics.cpp -I /tmp/klee/include

libkleeBasic-patch.a: Statistics.cpp.o
	cp /tmp/klee-build/lib/libkleeBasic.a ./libkleeBasic-patch.a
	ar vr libkleeBasic-patch.a Statistics.cpp.o

klee-patch: klee-patch.o libkleeBasic-patch.a
	g++ -o klee-patch klee-patch.o -L . -L /tmp/llvm-install/lib/ -L /tmp/klee-build/lib/ -L /tmp/z3_install/lib/ -lkleeCore -lz -ltcmalloc -lLLVMX86Disassembler -lLLVMX86AsmParser -lLLVMX86CodeGen -lLLVMCFGuard -lLLVMGlobalISel -lLLVMSelectionDAG -lLLVMAsmPrinter -lLLVMDebugInfoDWARF -lLLVMCodeGen -lLLVMScalarOpts -lLLVMInstCombine -lLLVMAggressiveInstCombine -lLLVMTransformUtils -lLLVMBitWriter -lLLVMX86Desc -lLLVMMCDisassembler -lLLVMX86Info -lLLVMMCJIT -lLLVMExecutionEngine -lLLVMTarget -lLLVMAnalysis -lLLVMProfileData -lLLVMRuntimeDyld -lLLVMObject -lLLVMTextAPI -lLLVMMCParser -lLLVMBitReader -lLLVMMC -lLLVMDebugInfoCodeView -lLLVMDebugInfoMSF -lLLVMCore -lLLVMRemarks -lLLVMBitstreamReader -lLLVMBinaryFormat -lLLVMSupport -lLLVMDemangle -lsqlite3 -lkleeBasic-patch -lLLVMSupport -lLLVMDemangle -lkleeModule -lLLVMipo -lLLVMInstrumentation -lLLVMVectorize -lLLVMLinker -lLLVMIRReader -lLLVMAsmParser -lLLVMFrontendOpenMP -lLLVMCodeGen -lLLVMTarget -lLLVMScalarOpts -lLLVMInstCombine -lLLVMAggressiveInstCombine -lLLVMTransformUtils -lLLVMBitWriter -lLLVMAnalysis -lLLVMProfileData -lLLVMObject -lLLVMTextAPI -lLLVMMCParser -lLLVMMC -lLLVMDebugInfoCodeView -lLLVMDebugInfoMSF -lLLVMBitReader -lLLVMCore -lLLVMRemarks -lLLVMBinaryFormat -lLLVMBitstreamReader -lLLVMSupport -lLLVMDemangle -lkleaverSolver -lkleaverExpr -lkleeSupport -lLLVMSupport -lLLVMDemangle -lpthread -ldl -lz3 -lz
	echo "To run the patched Klee binary: LD_LIBRARY_PATH=/tmp/z3_install/lib ./klee-patch"