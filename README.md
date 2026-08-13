# Illumina TruPath Genome WGS

![workflow](./workflow.png)

硬件：dragen sever v4

软件：dragen v4.5+

license:Proximity license（买了试剂盒免费送）

2.5-3.5h per WGS

## 名词解释

1. Proximity(临近)

 - Proximity rate：具有临近关系的reads占总reads的比例
 - link quality:基于Phred尺度的质量分数，用于评估两个临近reads来源于同一原始分子模版的概率
 - Link Q25 以上的reads，在全基因组上的平均覆盖度 > 30x (默认值:--proximity-min-linkq-threshold 10 --proximity-additional-linkq-thresholds 25)

2. Multi-Region Joint Detection (MRJD)多区域联合检测，只兼容hg38分析，涉及到15个医学相关的基因(9个医学相关的旁系同源区域进行小变异和拷贝数检测)
([15 clinically relevant genes](https://help.dragen.illumina.com/dragen-v4.5/product-guides/dragen-v4.5/dragen-trupath-pipeline#multi-region-joint-detection)).

![15_medically_relevant_paralogous_genes](./15_medically_relevant_paralogous_genes.png)

 - copy number is less than 8
 - only supports the hg38 reference genome
 - linked coverage (without duplicates) is ≥16X

[Input recommendations FAQ for the TruPath Genome assay](https://knowledge.illumina.com/library-preparation/general/library-preparation-general-faq-list/000010167)



![MRJD](MRJD.avif)

3. phasing(定相)

Phasing 统计结果文件:NG50 700kb-35Mb

Standard molecular weight DNA （SMW）vs High molecular weight （HMW）

![SMW_vs_HMW](./HMW_vs_SMW.png)

4. colocation协同定位(default size of 200 kb,默认输出文件：colocation.cooler) **third-party tools**
 - [cooler](https://cooler.readthedocs.io/en/latest/schema.html)  可以生成不同解析度的.mcooler
 - [HiGlass](https://github.com/higlass/higlass)    结果查看
 - [Visualize Data in HiGlass](https://help.connected.illumina.com/illumina-trupath-genome/data-visualizations-and-analysis/visualize-data-in-higlass)


## [DRAGEN Recipes:TruPath Pipelines](https://help.dragen.illumina.com/dragen-v4.5/product-guides/dragen-v4.5/dragen-recipes/illumina-trupath-genome-wgs)

    /opt/dragen/$VERSION/bin/dragen         #DRAGEN install path 
    --ref-dir $REF_DIR                      #path to DRAGEN pangenome hashtable 
    --output-directory $OUTPUT 
    --intermediate-results-dir $PATH        #e.g. SSD /staging 
    --output-file-prefix $PREFIX 
    # Inputs 
    --fastq-list $PATH                      #see 'Input Options' 
    --fastq-list-sample-id $STRING 
    # Illumina TruPath Genome 
    --enable-proximity true 
    # Mapper 
    --enable-map-align true                 #optional with BAM/CRAM input 
    --enable-map-align-output true          #optionally save the output BAM 
    --enable-sort true                      #default=true 
    --enable-duplicate-marking true         #default=true 
    # Small variant caller 
    --enable-variant-caller true 
    --vc-phasing-min-fragments 0 
    # SV 
    --enable-sv true 
    # CNV 
    --enable-cnv true 
    --cnv-enable-self-normalization true 
    # Targeted caller 
    --enable-targeted true 
    # Short tandem repeats 
    --repeat-genotype-enable true 
    # Multi-Region Joint Detection (MRJD) 
    --enable-mrjd true 

## [dragen-report && dragen-summary-reports](https://help.dragen.illumina.com/dragen-v4.5/product-guides/dragen-v4.5/dragen-reports)

下载rpm 软件包:

https://support.illumina.com/sequencing/sequencing_software/dragen-bio-it-platform/product_files.html

demo CMD:


     /usr/bin/dragen-reports -f \
    -d /data \ #dragen的输出结果文件
    -o /output/report.html \
    -m /opt/dragen-reports/manifests/trupath/germline_wgs.json


    /usr/bin/dragen-summary-reports \
        -f \
        -d /data \ #dragen的输出结果文件
        -o /data/AggregateReports/summary.html

## [TruPath Outputs:DRAGEN secondary analysis outputs and metrics](https://help.connected.illumina.com/illumina-trupath-genome/outputs-and-metrics/output-files-and-metrics)

## 参考链接

[Illumina TruPath Genome Pipeline:https://help.dragen.illumina.com/dragen-v4.5/product-guides/dragen-v4.5/dragen-trupath-pipeline](https://help.dragen.illumina.com/dragen-v4.5/product-guides/dragen-v4.5/dragen-trupath-pipeline)

[Illumina TruPath Genome Software Product Overview](https://help.connected.illumina.com/illumina-trupath-genome)

[Illumina TruPath Genome WGS:https://help.dragen.illumina.com/dragen-v4.5/product-guides/dragen-v4.5/dragen-recipes/illumina-trupath-genome-wgs](https://help.dragen.illumina.com/dragen-v4.5/product-guides/dragen-v4.5/dragen-recipes/illumina-trupath-genome-wgs)

[NovaSeq X Run Planning - BCL Convert](https://help.connected.illumina.com/illumina-trupath-genome/sequencing-run-setup/novaseq-x-run-planning-bcl-convert)

[Illumina TruPath Genome Product Documentation](https://support.illumina.com/sequencing/sequencing_kits/illumina-trupath-genome/documentation.html)

[Illumina TruPath Genome Support Resources](https://support.illumina.com/sequencing/sequencing_kits/illumina-trupath-genome.html)