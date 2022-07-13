# appendassemblymover
## 以生成talin蛋白为例

![image](https://user-images.githubusercontent.com/81453850/178770368-9896edae-5138-45d8-a789-ca8363930401.png)

##### 以下所有文件均在  /public3/home/pg3152/zhangxinyue/crispr/sewing/NuvC_HNH_HD/AppendAssemblyMover 路径下

### 准备segments文件(https://github.com/Raymundo-cj/rosetta-2021_test) ，原pdb结构(1sj7.pdb)，受体pdb (1sj7_t.pdb)，初始结构pdb(1sj7_p.pdb)
### ②准备xml文件与flag文件

#### xml文件内容如下  text.xml

<ROSETTASCRIPTS>
        <SCOREFXNS>
        </SCOREFXNS>
        <FILTERS>
        </FILTERS>
<MOVERS>
    <AppendAssemblyMover name="assemble"
      model_file_name="smotifs_H_3_19_L_1_23_H_3_31.segments"
      partner_pdb = "1sj7_t.pdb"
      hashed="false"
      minimum_cycles="1000"
      maximum_cycles="1100"
      required_resnums = "7,8,11,12"
      max_segments = "11"
      modifiable_terminus="B"
      output_partner = "true">
      <AssemblyScorers>
                <MotifScorer weight = "1" />
                <InterModelMotifScorer weight = "10" />
                <PartnerMotifScorer weight = "10" />
        </AssemblyScorers>
      <AssemblyRequirements>
        <DsspSpecificLengthRequirement dssp_code = "L" maximum_length = "5" />
        <ClashRequirement clash_radius = "4" />
      </AssemblyRequirements>
    </AppendAssemblyMover>
  </MOVERS>
        <PROTOCOLS>
                <Add mover_name="assemble" />
        </PROTOCOLS>
</ROSETTASCRIPTS>

#### flag文件内容如下

-ignore_unrecognized_res
-detect_disulf false
-mh
    -score
        -use_ss1 true
        -use_ss2 true
        -use_aa1 false
        -use_aa2 false
    -path
        -motifs /public3/home/pg3152/zzl/zzl_softwares/rosetta_src_2021.16.61629_bundle/main/database/additional_protocol_data/sewing/xsmax_bb_ss_AILV_resl0.8_msc0.3/xsmax_bb_ss_AILV_resl0.8_msc0.3.rpm.bin.gz
        -scores_BB_BB /public3/home/pg3152/zzl/zzl_softwares/rosetta_src_2021.16.61629_bundle/main/database/additional_protocol_data/sewing/xsmax_bb_ss_AILV_resl0.8_msc0.3/xsmax_bb_ss_AILV_resl0.8_msc0.3
    -gen_reverse_motifs_on_load false
    
### ③运行脚本内容

#!/bin/bash
/public3/home/pg3152/zzl/zzl_softwares/rosetta_src_2021.16.61629_bundle/main/source/bin/rosetta_scripts.linuxgccrelease -s 1sj7_p.pdb -parser:protocol test.xml @flag -nstruct 4 -out:path:pdb output


