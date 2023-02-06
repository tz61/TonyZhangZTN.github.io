在llvm/lib/Target 下 git clone llvm-lc3 为LC3后

并在`llvm/CMakeLists.txt`下line 368 添加LC3到LLVM_ALL_TARGETS后

出现如下报错

```bash
CMake Error at cmake/modules/LLVM-Build.cmake:42 (get_property):
  get_property could not find TARGET LC3.  Perhaps it has not yet been
  created.
Call Stack (most recent call first):
  lib/CMakeLists.txt:65 (LLVMBuildGenerateCFragment)


CMake Error at cmake/modules/LLVM-Build.cmake:43 (get_property):
  get_property could not find TARGET LC3.  Perhaps it has not yet been
  created.
Call Stack (most recent call first):
  lib/CMakeLists.txt:65 (LLVMBuildGenerateCFragment)
```

于是展开进一步分析

在LC3/`CMakeLists.txt` 中添加`add_llvm_component_group(LC3)` 新的llvm cmake 机制不一样，导致识别不了

老版本:在llvm/lib/Target/LLVMBuild.txt 中添加LC3

解决依赖后，编译报错

```textile
FAILED: lib/Target/LC3/LC3GenDAGISel.inc /home/ztn/llvm-project/build/lib/Target/LC3/LC3GenDAGISel.inc
cd /home/ztn/llvm-project/build && /home/ztn/llvm-project/build/bin/llvm-tblgen -gen-dag-isel -I /home/ztn/llvm-project/llvm/lib/Target/LC3 -I/home/ztn/llvm-project/build/include -I/home/ztn/llvm-project/llvm/include -I /home/ztn/llvm-project/llvm/lib/Target -omit-comments /home/ztn/llvm-project/llvm/lib/Target/LC3/LC3.td --write-if-changed -o lib/Target/LC3/LC3GenDAGISel.inc -d lib/Target/LC3/LC3GenDAGISel.inc.d
ADJCALLSTACKDOWN:       (callseq_start (timm:{ *:[i16] }):$amt)
Included from /home/ztn/llvm-project/llvm/lib/Target/LC3/LC3.td:25:
/home/ztn/llvm-project/llvm/lib/Target/LC3/LC3InstrInfo.td:270:1: error: In ADJCALLSTACKDOWN: callseq_start node requires exactly 2 operands!
def ADJCALLSTACKDOWN : LC3PseudoInst<0b1011, (outs), (ins i16imm:$amt),
^
vtInt:  (vt:{ *:[Other] })
Included from /home/ztn/llvm-project/llvm/lib/Target/LC3/LC3.td:18:
Included from /home/ztn/llvm-project/llvm/include/llvm/Target/Target.td:1777:
/home/ztn/llvm-project/llvm/include/llvm/Target/TargetSelectionDAG.td:950:1: error: In vtInt: callseq_start node requires exactly 2 operands!
def vtInt      : PatLeaf<(vt),  [{ return N->getVT().isInteger(); }]>;
^
/home/ztn/llvm-project/build/bin/llvm-tblgen: 2 errors.
[2589/4588] Building LC3GenInstrInfo.inc...
FAILED: lib/Target/LC3/LC3GenInstrInfo.inc /home/ztn/llvm-project/build/lib/Target/LC3/LC3GenInstrInfo.inc
cd /home/ztn/llvm-project/build && /home/ztn/llvm-project/build/bin/llvm-tblgen -gen-instr-info -I /home/ztn/llvm-project/llvm/lib/Target/LC3 -I/home/ztn/llvm-project/build/include -I/home/ztn/llvm-project/llvm/include -I /home/ztn/llvm-project/llvm/lib/Target /home/ztn/llvm-project/llvm/lib/Target/LC3/LC3.td --write-if-changed -o lib/Target/LC3/LC3GenInstrInfo.inc -d lib/Target/LC3/LC3GenInstrInfo.inc.d
ADJCALLSTACKDOWN:       (callseq_start (timm:{ *:[i16] }):$amt)
Included from /home/ztn/llvm-project/llvm/lib/Target/LC3/LC3.td:25:
/home/ztn/llvm-project/llvm/lib/Target/LC3/LC3InstrInfo.td:270:1: error: In ADJCALLSTACKDOWN: callseq_start node requires exactly 2 operands!
def ADJCALLSTACKDOWN : LC3PseudoInst<0b1011, (outs), (ins i16imm:$amt),
```

尝试降低版本： 



3.8:



3.7:

error: ‘lc3’ is not a member of ‘llvm::Triple’ 

llvm-3.7.0.src/lib/Target/LC3/LC3MCInstLower.cpp:78:31: error: ‘VK_LC3_HI’ is not a member of ‘llvm::MCSymbolRefExpr’

llvm-3.7.0.src/lib/Target/LC3/LC3MCInstLower.cpp:75:31: error: ‘VK_LC3_LO’ is not a member of ‘llvm::MCSymbolRefExpr’

....

3.6:

cannot declare field ‘llvm::LC3Subtarget::FrameLowering’ to be of abstract type ‘llvm::LC3FrameLowering’

‘void llvm::LC3FrameLowering::emitPrologue(llvm::MachineFunction&, llvm::MachineBasicBlock&) const’ marked ‘override’, but does not override

3.5:

error: base operand of ‘->’ has non-pointer type ‘llvm::MCStreamer’

3.4:

error: ‘class llvm::SelectionDAG’ has no member named ‘getDataLayout’
