# Pl-NCHU-NPUST-175

*Purpureocillium lilacinum* NCHU-NPUST-175 基因體分析流程 (pipeline) 整理。
內容涵蓋從 Nanopore 定序資料的組裝、基因注釋、比較基因體到親緣關係樹建構。各步驟的完整指令請見對應資料夾中的 README。

## 關於本研究

*P. lilacinum* NCHU-NPUST-175 是一株具有防治台灣鋏蠓 (*Forcipomyia taiwana*) 潛力的昆蟲病原真菌。本研究以 Oxford Nanopore 長讀長定序取得該菌株的基因體，並透過比較基因體分析找出菌株與物種特異性的 DNA 區域 (distinctive genomic regions, DGRs)，據此開發可用於環境偵測與田間施用前身分確認的分子標記。


## 資料夾結構

```
Pl-NCHU-NPUST-175
├── assembly/            # 基因體與粒線體組裝、品質評估
├── annotation/          # 重複序列、基因預測與功能注釋
├── comparative genome/  # 比較基因體與特異性區域(DGRs)分析
├── phylogenetic/        # 基因體與粒線體親緣關係樹
└── README.md
```

## 分析流程總覽

```
assembly ──► annotation ──► comparative genome
    │
    └──────────────────────► phylogenetic
```

| 資料夾 | 內容 | 主要工具 |
| --- | --- | --- |
| [assembly](./assembly/README.md) | Basecalling、去接頭、de novo 組裝、polish、組裝品質與完整度評估、粒線體基因體組裝 | Guppy, Porechop, Canu, Nanopolish, BWA, SAMtools, QUAST, BUSCO, NanoPlot, Circlator |
| [annotation](./annotation/README.md) | 重複序列與轉位子(TE)分析、基因預測、功能注釋、rRNA 預測 | RepeatModeler, RepeatMasker, FunGAP, BUSCO, InterProScan, PHI-base BLAST, dbCAN, antiSMASH, Barrnap |
| [comparative genome](./comparative%20genome/README.md) | 與參考基因體比對、找出特異性區域 (DGRs) 並以 BLAST 進一步驗證 | MUMmer, SeqKit, BEDTools, BLAST+ |
| [phylogenetic](./phylogenetic/README.md) | 全基因體 (核心基因) 樹與粒線體基因體樹 | UFCG, MAFFT, PartitionFinder, RAxML, IQ-TREE |


## 引用

Yeh Z.-Y., Mushyakhwo K., Ni N.-T., Lin P.-H., Lin C.-F., Nai Y.-S. Genomic analyses of *Purpureocillium lilacinum* reveal unique DNA regions for developing isolate-specific molecular markers. *BMC Genomics* 26, 843 (2025). https://doi.org/10.1186/s12864-025-12018-6