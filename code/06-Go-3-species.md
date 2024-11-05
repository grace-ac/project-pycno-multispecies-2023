06-Go-3-species
================
2024-11-05

Lets get GO annotations for der, pis, pyc….

# der

    fasta="../output/01-data-explore/trinity-der/der-trinity.fa"

    blastx \
    -query $fasta \
    -db ../data/blastdb/uniprot_sprot_r2024_05 \
    -out ../output/02-blast-klonetest/der_blastx_sp.tab \
    -evalue 1E-05 \
    -num_threads 30 \
    -max_target_seqs 1 \
    -max_hsps 1 \
    -outfmt 6

``` bash
head ../output/02-blast-klonetest/der_blastx_sp.tab
```

    ## TRINITY_DN37009_c0_g3_i1 sp|Q9BYN0|SRXN1_HUMAN   57.600  125 47  2   2822    2448    18  136 1.11e-39    147
    ## TRINITY_DN37009_c0_g3_i3 sp|Q9BYN0|SRXN1_HUMAN   57.600  125 47  2   1720    1346    18  136 3.47e-40    147
    ## TRINITY_DN37035_c0_g1_i1 sp|P21329|RTJK_DROFU    28.571  147 92  4   169 579 613 756 4.94e-08    55.8
    ## TRINITY_DN37023_c0_g1_i1 sp|P07700|ADRB1_MELGA   33.333  99  65  1   47  340 59  157 6.59e-09    55.5
    ## TRINITY_DN37023_c1_g1_i1 sp|P07700|ADRB1_MELGA   31.579  133 87  3   473 84  44  175 9.86e-12    66.6
    ## TRINITY_DN45808_c0_g1_i1 sp|P52732|KIF11_HUMAN   57.143  49  21  0   85  231 374 422 2.25e-10    58.9
    ## TRINITY_DN45839_c0_g1_i1 sp|Q6AX60|PTHB1_XENLA   38.889  90  47  3   15  275 425 509 3.79e-07    49.7
    ## TRINITY_DN45781_c0_g1_i1 sp|Q8CIQ6|MTR1B_MOUSE   27.374  179 105 7   1   513 85  246 6.95e-07    51.6
    ## TRINITY_DN45840_c0_g1_i1 sp|P07911|UROM_HUMAN    42.353  85  44  2   104 343 64  148 2.25e-13    69.3
    ## TRINITY_DN48095_c0_g1_i1 sp|P0C0R5|PI3R4_RAT 54.054  37  13  1   102 4   1097    1133    4.19e-06    46.2

``` bash
tr '|' '\t' < ../output/02-blast-klonetest/der_blastx_sp.tab \
> ../output/06-Go-3-species/der_blastx_sep.tab

head -1 ../output/06-Go-3-species/der_blastx_sep.tab
```

    ## TRINITY_DN37009_c0_g3_i1 sp  Q9BYN0  SRXN1_HUMAN 57.600  125 47  2   2822    2448    18  136 1.11e-39    147

join

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
derbl <- read_tsv("../output/06-Go-3-species/der_blastx_sep.tab", col_names = FALSE)
```

    ## Rows: 34254 Columns: 14
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: "\t"
    ## chr  (4): X1, X2, X3, X4
    ## dbl (10): X5, X6, X7, X8, X9, X10, X11, X12, X13, X14
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
sp_go <- read_tsv("../data/SwissProt-Annot.tsv", col_names = TRUE)
```

    ## Rows: 145492 Columns: 12
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: "\t"
    ## chr (11): Entry, Reviewed, Entry Name, Protein names, Gene Names, Organism, ...
    ## dbl  (1): Length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
der_go <- left_join(derbl, sp_go, by = c("X3" = "Entry"))
```

``` r
write_tsv(der_go, "../output/06-Go-3-species/der_go.tsv")
```

``` bash
head ../output/06-Go-3-species/der_go.tsv
```

    ## X1   X2  X3  X4  X5  X6  X7  X8  X9  X10 X11 X12 X13 X14 Reviewed    Entry Name  Protein names   Gene Names  Organism    Length  Gene Ontology (biological process)  Gene Ontology (cellular component)  Gene Ontology (GO)  Gene Ontology (molecular function)  Gene Ontology IDs
    ## TRINITY_DN37009_c0_g3_i1 sp  Q9BYN0  SRXN1_HUMAN 57.6    125 47  2   2822    2448    18  136 1.1099999999999999e-39  147 reviewed    SRXN1_HUMAN Sulfiredoxin-1 (EC 1.8.98.2)    SRXN1 C20orf139 SRX SRX1    Homo sapiens (Human)    137 cellular response to oxidative stress [GO:0034599]; response to oxidative stress [GO:0006979]   cytoplasm [GO:0005737]; cytosol [GO:0005829]; endoplasmic reticulum membrane [GO:0005789]   cytoplasm [GO:0005737]; cytosol [GO:0005829]; endoplasmic reticulum membrane [GO:0005789]; ATP binding [GO:0005524]; oxidoreductase activity, acting on a sulfur group of donors [GO:0016667]; sulfiredoxin activity [GO:0032542]; cellular response to oxidative stress [GO:0034599]; response to oxidative stress [GO:0006979]    ATP binding [GO:0005524]; oxidoreductase activity, acting on a sulfur group of donors [GO:0016667]; sulfiredoxin activity [GO:0032542]  GO:0005524; GO:0005737; GO:0005789; GO:0005829; GO:0006979; GO:0016667; GO:0032542; GO:0034599
    ## TRINITY_DN37009_c0_g3_i3 sp  Q9BYN0  SRXN1_HUMAN 57.6    125 47  2   1720    1346    18  136 3.47e-40    147 reviewed    SRXN1_HUMAN Sulfiredoxin-1 (EC 1.8.98.2)    SRXN1 C20orf139 SRX SRX1    Homo sapiens (Human)    137 cellular response to oxidative stress [GO:0034599]; response to oxidative stress [GO:0006979]   cytoplasm [GO:0005737]; cytosol [GO:0005829]; endoplasmic reticulum membrane [GO:0005789]   cytoplasm [GO:0005737]; cytosol [GO:0005829]; endoplasmic reticulum membrane [GO:0005789]; ATP binding [GO:0005524]; oxidoreductase activity, acting on a sulfur group of donors [GO:0016667]; sulfiredoxin activity [GO:0032542]; cellular response to oxidative stress [GO:0034599]; response to oxidative stress [GO:0006979]    ATP binding [GO:0005524]; oxidoreductase activity, acting on a sulfur group of donors [GO:0016667]; sulfiredoxin activity [GO:0032542]  GO:0005524; GO:0005737; GO:0005789; GO:0005829; GO:0006979; GO:0016667; GO:0032542; GO:0034599
    ## TRINITY_DN37035_c0_g1_i1 sp  P21329  RTJK_DROFU  28.571  147 92  4   169 579 613 756 4.94e-8 55.8    NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN37023_c0_g1_i1 sp  P07700  ADRB1_MELGA 33.333  99  65  1   47  340 59  157 6.59e-9 55.5    reviewed    ADRB1_MELGA Beta-1 adrenergic receptor (Beta-1 adrenoreceptor) (Beta-1 adrenoceptor) (Beta-T)   ADRB1   Meleagris gallopavo (Wild turkey)   483 adenylate cyclase-activating adrenergic receptor signaling pathway [GO:0071880]; positive regulation of heart contraction [GO:0045823]; regulation of circadian sleep/wake cycle, sleep [GO:0045187]    early endosome [GO:0005769]; membrane [GO:0016020]; plasma membrane [GO:0005886]    early endosome [GO:0005769]; membrane [GO:0016020]; plasma membrane [GO:0005886]; beta1-adrenergic receptor activity [GO:0004940]; identical protein binding [GO:0042802]; adenylate cyclase-activating adrenergic receptor signaling pathway [GO:0071880]; positive regulation of heart contraction [GO:0045823]; regulation of circadian sleep/wake cycle, sleep [GO:0045187] beta1-adrenergic receptor activity [GO:0004940]; identical protein binding [GO:0042802] GO:0004940; GO:0005769; GO:0005886; GO:0016020; GO:0042802; GO:0045187; GO:0045823; GO:0071880
    ## TRINITY_DN37023_c1_g1_i1 sp  P07700  ADRB1_MELGA 31.579  133 87  3   473 84  44  175 9.86e-12    66.6    reviewed    ADRB1_MELGA Beta-1 adrenergic receptor (Beta-1 adrenoreceptor) (Beta-1 adrenoceptor) (Beta-T)   ADRB1   Meleagris gallopavo (Wild turkey)   483 adenylate cyclase-activating adrenergic receptor signaling pathway [GO:0071880]; positive regulation of heart contraction [GO:0045823]; regulation of circadian sleep/wake cycle, sleep [GO:0045187]    early endosome [GO:0005769]; membrane [GO:0016020]; plasma membrane [GO:0005886]    early endosome [GO:0005769]; membrane [GO:0016020]; plasma membrane [GO:0005886]; beta1-adrenergic receptor activity [GO:0004940]; identical protein binding [GO:0042802]; adenylate cyclase-activating adrenergic receptor signaling pathway [GO:0071880]; positive regulation of heart contraction [GO:0045823]; regulation of circadian sleep/wake cycle, sleep [GO:0045187] beta1-adrenergic receptor activity [GO:0004940]; identical protein binding [GO:0042802] GO:0004940; GO:0005769; GO:0005886; GO:0016020; GO:0042802; GO:0045187; GO:0045823; GO:0071880
    ## TRINITY_DN45808_c0_g1_i1 sp  P52732  KIF11_HUMAN 57.143  49  21  0   85  231 374 422 2.25e-10    58.9    reviewed    KIF11_HUMAN Kinesin-like protein KIF11 (Kinesin-like protein 1) (Kinesin-like spindle protein HKSP) (Kinesin-related motor protein Eg5) (Thyroid receptor-interacting protein 5) (TR-interacting protein 5) (TRIP-5)    KIF11 EG5 KNSL1 TRIP5   Homo sapiens (Human)    1056    cell division [GO:0051301]; microtubule-based movement [GO:0007018]; mitotic cell cycle [GO:0000278]; mitotic centrosome separation [GO:0007100]; mitotic spindle assembly [GO:0090307]; mitotic spindle organization [GO:0007052]; regulation of mitotic centrosome separation [GO:0046602]; spindle elongation [GO:0051231]; spindle organization [GO:0007051]    cytosol [GO:0005829]; kinesin complex [GO:0005871]; membrane [GO:0016020]; microtubule [GO:0005874]; mitotic spindle [GO:0072686]; nucleus [GO:0005634]; protein-containing complex [GO:0032991]; spindle [GO:0005819]; spindle pole [GO:0000922]   cytosol [GO:0005829]; kinesin complex [GO:0005871]; membrane [GO:0016020]; microtubule [GO:0005874]; mitotic spindle [GO:0072686]; nucleus [GO:0005634]; protein-containing complex [GO:0032991]; spindle [GO:0005819]; spindle pole [GO:0000922]; ATP binding [GO:0005524]; microtubule binding [GO:0008017]; microtubule motor activity [GO:0003777]; plus-end-directed microtubule motor activity [GO:0008574]; protein kinase binding [GO:0019901]; cell division [GO:0051301]; microtubule-based movement [GO:0007018]; mitotic cell cycle [GO:0000278]; mitotic centrosome separation [GO:0007100]; mitotic spindle assembly [GO:0090307]; mitotic spindle organization [GO:0007052]; regulation of mitotic centrosome separation [GO:0046602]; spindle elongation [GO:0051231]; spindle organization [GO:0007051]    ATP binding [GO:0005524]; microtubule binding [GO:0008017]; microtubule motor activity [GO:0003777]; plus-end-directed microtubule motor activity [GO:0008574]; protein kinase binding [GO:0019901] GO:0000278; GO:0000922; GO:0003777; GO:0005524; GO:0005634; GO:0005819; GO:0005829; GO:0005871; GO:0005874; GO:0007018; GO:0007051; GO:0007052; GO:0007100; GO:0008017; GO:0008574; GO:0016020; GO:0019901; GO:0032991; GO:0046602; GO:0051231; GO:0051301; GO:0072686; GO:0090307
    ## TRINITY_DN45839_c0_g1_i1 sp  Q6AX60  PTHB1_XENLA 38.889  90  47  3   15  275 425 509 3.79e-7 49.7    NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN45781_c0_g1_i1 sp  Q8CIQ6  MTR1B_MOUSE 27.374  179 105 7   1   513 85  246 6.95e-7 51.6    reviewed    MTR1B_MOUSE Melatonin receptor type 1B (Mel-1B-R) (Mel1b receptor)  Mtnr1b  Mus musculus (Mouse)    364 circadian rhythm [GO:0007623]; G protein-coupled receptor signaling pathway [GO:0007186]; negative regulation of cGMP-mediated signaling [GO:0010754]; negative regulation of cytosolic calcium ion concentration [GO:0051481]; negative regulation of insulin secretion [GO:0046676]; negative regulation of neuron apoptotic process [GO:0043524]; negative regulation of transmission of nerve impulse [GO:0051970]; negative regulation of vasoconstriction [GO:0045906]; positive regulation of circadian rhythm [GO:0042753]; positive regulation of circadian sleep/wake cycle, non-REM sleep [GO:0046010]; positive regulation of transmission of nerve impulse [GO:0051971]; regulation of neuronal action potential [GO:0098908]  plasma membrane [GO:0005886]    plasma membrane [GO:0005886]; G protein-coupled receptor activity [GO:0004930]; melatonin receptor activity [GO:0008502]; circadian rhythm [GO:0007623]; G protein-coupled receptor signaling pathway [GO:0007186]; negative regulation of cGMP-mediated signaling [GO:0010754]; negative regulation of cytosolic calcium ion concentration [GO:0051481]; negative regulation of insulin secretion [GO:0046676]; negative regulation of neuron apoptotic process [GO:0043524]; negative regulation of transmission of nerve impulse [GO:0051970]; negative regulation of vasoconstriction [GO:0045906]; positive regulation of circadian rhythm [GO:0042753]; positive regulation of circadian sleep/wake cycle, non-REM sleep [GO:0046010]; positive regulation of transmission of nerve impulse [GO:0051971]; regulation of neuronal action potential [GO:0098908]    G protein-coupled receptor activity [GO:0004930]; melatonin receptor activity [GO:0008502]  GO:0004930; GO:0005886; GO:0007186; GO:0007623; GO:0008502; GO:0010754; GO:0042753; GO:0043524; GO:0045906; GO:0046010; GO:0046676; GO:0051481; GO:0051970; GO:0051971; GO:0098908
    ## TRINITY_DN45840_c0_g1_i1 sp  P07911  UROM_HUMAN  42.353  85  44  2   104 343 64  148 2.25e-13    69.3    reviewed    UROM_HUMAN  Uromodulin (Tamm-Horsfall urinary glycoprotein) (THP) [Cleaved into: Uromodulin, secreted form] UMOD    Homo sapiens (Human)    640 antibacterial innate immune response [GO:0140367]; apoptotic signaling pathway [GO:0097190]; autophagy [GO:0006914]; cellular defense response [GO:0006968]; cellular response to unfolded protein [GO:0034620]; chaperone-mediated protein folding [GO:0061077]; citric acid secretion [GO:0046720]; collecting duct development [GO:0072044]; connective tissue replacement [GO:0097709]; defense response to Gram-negative bacterium [GO:0050829]; endoplasmic reticulum organization [GO:0007029]; ERAD pathway [GO:0036503]; glomerular filtration [GO:0003094]; heterophilic cell-cell adhesion via plasma membrane cell adhesion molecules [GO:0007157]; inflammatory response [GO:0006954]; intracellular calcium ion homeostasis [GO:0006874]; intracellular chloride ion homeostasis [GO:0030644]; intracellular phosphate ion homeostasis [GO:0030643]; intracellular sodium ion homeostasis [GO:0006883]; juxtaglomerular apparatus development [GO:0072051]; leukocyte cell-cell adhesion [GO:0007159]; lipid metabolic process [GO:0006629]; metanephric ascending thin limb development [GO:0072218]; metanephric distal convoluted tubule development [GO:0072221]; metanephric thick ascending limb development [GO:0072233]; micturition [GO:0060073]; multicellular organismal response to stress [GO:0033555]; negative regulation of cell population proliferation [GO:0008285]; neutrophil migration [GO:1990266]; organ or tissue specific immune response [GO:0002251]; potassium ion homeostasis [GO:0055075]; protein localization to vacuole [GO:0072665]; protein transport into plasma membrane raft [GO:0044861]; regulation of blood pressure [GO:0008217]; regulation of protein transport [GO:0051223]; regulation of urine volume [GO:0035809]; renal sodium ion absorption [GO:0070294]; renal urate salt excretion [GO:0097744]; renal water homeostasis [GO:0003091]; response to lipopolysaccharide [GO:0032496]; response to water deprivation [GO:0009414]; response to xenobiotic stimulus [GO:0009410]; RNA splicing [GO:0008380]; tumor necrosis factor-mediated signaling pathway [GO:0033209]; urate transport [GO:0015747]; urea transmembrane transport [GO:0071918] apical plasma membrane [GO:0016324]; basolateral plasma membrane [GO:0016323]; cell surface [GO:0009986]; ciliary membrane [GO:0060170]; cilium [GO:0005929]; endoplasmic reticulum [GO:0005783]; extracellular exosome [GO:0070062]; extracellular space [GO:0005615]; extrinsic component of membrane [GO:0019898]; Golgi lumen [GO:0005796]; membrane [GO:0016020]; side of membrane [GO:0098552]; spindle pole [GO:0000922] apical plasma membrane [GO:0016324]; basolateral plasma membrane [GO:0016323]; cell surface [GO:0009986]; ciliary membrane [GO:0060170]; cilium [GO:0005929]; endoplasmic reticulum [GO:0005783]; extracellular exosome [GO:0070062]; extracellular space [GO:0005615]; extrinsic component of membrane [GO:0019898]; Golgi lumen [GO:0005796]; membrane [GO:0016020]; side of membrane [GO:0098552]; spindle pole [GO:0000922]; calcium ion binding [GO:0005509]; IgG binding [GO:0019864]; antibacterial innate immune response [GO:0140367]; apoptotic signaling pathway [GO:0097190]; autophagy [GO:0006914]; cellular defense response [GO:0006968]; cellular response to unfolded protein [GO:0034620]; chaperone-mediated protein folding [GO:0061077]; citric acid secretion [GO:0046720]; collecting duct development [GO:0072044]; connective tissue replacement [GO:0097709]; defense response to Gram-negative bacterium [GO:0050829]; endoplasmic reticulum organization [GO:0007029]; ERAD pathway [GO:0036503]; glomerular filtration [GO:0003094]; heterophilic cell-cell adhesion via plasma membrane cell adhesion molecules [GO:0007157]; inflammatory response [GO:0006954]; intracellular calcium ion homeostasis [GO:0006874]; intracellular chloride ion homeostasis [GO:0030644]; intracellular phosphate ion homeostasis [GO:0030643]; intracellular sodium ion homeostasis [GO:0006883]; juxtaglomerular apparatus development [GO:0072051]; leukocyte cell-cell adhesion [GO:0007159]; lipid metabolic process [GO:0006629]; metanephric ascending thin limb development [GO:0072218]; metanephric distal convoluted tubule development [GO:0072221]; metanephric thick ascending limb development [GO:0072233]; micturition [GO:0060073]; multicellular organismal response to stress [GO:0033555]; negative regulation of cell population proliferation [GO:0008285]; neutrophil migration [GO:1990266]; organ or tissue specific immune response [GO:0002251]; potassium ion homeostasis [GO:0055075]; protein localization to vacuole [GO:0072665]; protein transport into plasma membrane raft [GO:0044861]; regulation of blood pressure [GO:0008217]; regulation of protein transport [GO:0051223]; regulation of urine volume [GO:0035809]; renal sodium ion absorption [GO:0070294]; renal urate salt excretion [GO:0097744]; renal water homeostasis [GO:0003091]; response to lipopolysaccharide [GO:0032496]; response to water deprivation [GO:0009414]; response to xenobiotic stimulus [GO:0009410]; RNA splicing [GO:0008380]; tumor necrosis factor-mediated signaling pathway [GO:0033209]; urate transport [GO:0015747]; urea transmembrane transport [GO:0071918]    calcium ion binding [GO:0005509]; IgG binding [GO:0019864]  GO:0000922; GO:0002251; GO:0003091; GO:0003094; GO:0005509; GO:0005615; GO:0005783; GO:0005796; GO:0005929; GO:0006629; GO:0006874; GO:0006883; GO:0006914; GO:0006954; GO:0006968; GO:0007029; GO:0007157; GO:0007159; GO:0008217; GO:0008285; GO:0008380; GO:0009410; GO:0009414; GO:0009986; GO:0015747; GO:0016020; GO:0016323; GO:0016324; GO:0019864; GO:0019898; GO:0030643; GO:0030644; GO:0032496; GO:0033209; GO:0033555; GO:0034620; GO:0035809; GO:0036503; GO:0044861; GO:0046720; GO:0050829; GO:0051223; GO:0055075; GO:0060073; GO:0060170; GO:0061077; GO:0070062; GO:0070294; GO:0071918; GO:0072044; GO:0072051; GO:0072218; GO:0072221; GO:0072233; GO:0072665; GO:0097190; GO:0097709; GO:0097744; GO:0098552; GO:0140367; GO:1990266

# pyc

    fasta="../output/01-data-explore/trinity-pyc/pyc_trinity.fa"

    /home/shared/ncbi-blast-2.15.0+/bin/blastx \
    -query $fasta \
    -db ../data/blastdb/uniprot_sprot_r2024_05 \
    -out ../output/01-data-explore/pyc_blastx_sp.tab \
    -evalue 1E-05 \
    -num_threads 20 \
    -max_target_seqs 1 \
    -max_hsps 1 \
    -outfmt 6

``` bash
head ../output/01-data-explore/pyc_blastx_sp.tab
```

    ## TRINITY_DN37046_c0_g1_i2 sp|Q32NR4|TTC29_XENLA   36.446  439 269 5   502 1818    19  447 6.37e-79    263
    ## TRINITY_DN37065_c0_g1_i1 sp|Q95NT6|MTH2_DROYA    25.974  231 155 8   688 29  224 449 2.22e-13    75.5
    ## TRINITY_DN37015_c0_g1_i1 sp|Q9Z0Z4|HEPH_MOUSE    47.500  80  42  0   2   241 38  117 3.37e-21    89.0
    ## TRINITY_DN45769_c0_g1_i1 sp|Q09575|YRD6_CAEEL    37.190  121 75  1   361 2   776 896 4.39e-25    102
    ## TRINITY_DN45794_c0_g1_i1 sp|Q9Y493|ZAN_HUMAN 51.724  58  28  0   12  185 1146    1203    1.02e-14    70.1
    ## TRINITY_DN45757_c0_g1_i1 sp|A1DWM3|MFSD6_PIG 40.000  60  35  1   217 38  445 503 8.34e-09    55.8
    ## TRINITY_DN45781_c0_g1_i1 sp|Q8WPA2|AR_BOMMO  51.429  35  17  0   55  159 297 331 8.20e-06    45.8
    ## TRINITY_DN45797_c0_g1_i1 sp|P0CT40|TF29_SCHPO    31.818  88  59  1   86  349 456 542 5.13e-07    50.1
    ## TRINITY_DN45764_c0_g1_i1 sp|P54360|FOJO_DROME    43.590  78  42  1   274 41  437 512 2.65e-11    62.8
    ## TRINITY_DN45800_c0_g1_i1 sp|Q46635|AMSE_ERWAM    36.364  88  45  3   266 12  34  113 1.09e-06    47.8

``` bash
tr '|' '\t' < ../output/01-data-explore/pyc_blastx_sp.tab \
> ../output/06-Go-3-species/pyc_blastx_sep.tab

head -1 ../output/06-Go-3-species/pyc_blastx_sep.tab
```

    ## TRINITY_DN37046_c0_g1_i2 sp  Q32NR4  TTC29_XENLA 36.446  439 269 5   502 1818    19  447 6.37e-79    263

join

``` r
library(tidyverse)

pycbl <- read_tsv("../output/06-Go-3-species/pyc_blastx_sep.tab", col_names = FALSE)
```

    ## Rows: 29145 Columns: 14
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: "\t"
    ## chr  (4): X1, X2, X3, X4
    ## dbl (10): X5, X6, X7, X8, X9, X10, X11, X12, X13, X14
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
sp_go <- read_tsv("../data/SwissProt-Annot.tsv", col_names = TRUE)
```

    ## Rows: 145492 Columns: 12
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: "\t"
    ## chr (11): Entry, Reviewed, Entry Name, Protein names, Gene Names, Organism, ...
    ## dbl  (1): Length
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pyc_go <- left_join(pycbl, sp_go, by = c("X3" = "Entry"))
```

``` r
write_tsv(pyc_go, "../output/06-Go-3-species/pyc_go.tsv")
```

``` bash
head ../output/06-Go-3-species/pyc_go.tsv
```

    ## X1   X2  X3  X4  X5  X6  X7  X8  X9  X10 X11 X12 X13 X14 Reviewed    Entry Name  Protein names   Gene Names  Organism    Length  Gene Ontology (biological process)  Gene Ontology (cellular component)  Gene Ontology (GO)  Gene Ontology (molecular function)  Gene Ontology IDs
    ## TRINITY_DN37046_c0_g1_i2 sp  Q32NR4  TTC29_XENLA 36.446  439 269 5   502 1818    19  447 6.37e-79    263 NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN37065_c0_g1_i1 sp  Q95NT6  MTH2_DROYA  25.974  231 155 8   688 29  224 449 2.22e-13    75.5    NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN37015_c0_g1_i1 sp  Q9Z0Z4  HEPH_MOUSE  47.5    80  42  0   2   241 38  117 3.3700000000000004e-21  89  reviewed    HEPH_MOUSE  Hephaestin (Hp) (EC 1.16.3.1)   Heph Kiaa0698   Mus musculus (Mouse)    1157    intestinal iron absorption [GO:0160179]; iron ion transport [GO:0006826]; multicellular organismal-level iron ion homeostasis [GO:0060586]; positive regulation of iron export across plasma membrane [GO:1904040]  basolateral plasma membrane [GO:0016323]; extracellular region [GO:0005576]; perinuclear region of cytoplasm [GO:0048471]; plasma membrane [GO:0005886] basolateral plasma membrane [GO:0016323]; extracellular region [GO:0005576]; perinuclear region of cytoplasm [GO:0048471]; plasma membrane [GO:0005886]; copper ion binding [GO:0005507]; ferrous iron binding [GO:0008198]; ferroxidase activity [GO:0004322]; oxidoreductase activity [GO:0016491]; intestinal iron absorption [GO:0160179]; iron ion transport [GO:0006826]; multicellular organismal-level iron ion homeostasis [GO:0060586]; positive regulation of iron export across plasma membrane [GO:1904040]    copper ion binding [GO:0005507]; ferrous iron binding [GO:0008198]; ferroxidase activity [GO:0004322]; oxidoreductase activity [GO:0016491] GO:0004322; GO:0005507; GO:0005576; GO:0005886; GO:0006826; GO:0008198; GO:0016323; GO:0016491; GO:0048471; GO:0060586; GO:0160179; GO:1904040
    ## TRINITY_DN45769_c0_g1_i1 sp  Q09575  YRD6_CAEEL  37.19   121 75  1   361 2   776 896 4.3899999999999995e-25  102 NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN45794_c0_g1_i1 sp  Q9Y493  ZAN_HUMAN   51.724  58  28  0   12  185 1146    1203    1.02e-14    70.1    reviewed    ZAN_HUMAN   Zonadhesin  ZAN Homo sapiens (Human)    2812    binding of sperm to zona pellucida [GO:0007339]; cell-cell adhesion [GO:0098609]    extracellular matrix [GO:0031012]; extracellular space [GO:0005615]; plasma membrane [GO:0005886]   extracellular matrix [GO:0031012]; extracellular space [GO:0005615]; plasma membrane [GO:0005886]; binding of sperm to zona pellucida [GO:0007339]; cell-cell adhesion [GO:0098609] NA  GO:0005615; GO:0005886; GO:0007339; GO:0031012; GO:0098609
    ## TRINITY_DN45757_c0_g1_i1 sp  A1DWM3  MFSD6_PIG   40  60  35  1   217 38  445 503 8.34e-9 55.8    NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN45781_c0_g1_i1 sp  Q8WPA2  AR_BOMMO    51.429  35  17  0   55  159 297 331 8.2e-6  45.8    NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN45797_c0_g1_i1 sp  P0CT40  TF29_SCHPO  31.818  88  59  1   86  349 456 542 5.13e-7 50.1    NA  NA  NA  NA  NA  NA  NA  NA  NA  NA  NA
    ## TRINITY_DN45764_c0_g1_i1 sp  P54360  FOJO_DROME  43.59   78  42  1   274 41  437 512 2.65e-11    62.8    reviewed    FOJO_DROME  Extracellular serine/threonine protein kinase four-jointed (EC 2.7.11.1) [Cleaved into: Protein four-jointed, secreted isoform] fj CG10917  Drosophila melanogaster (Fruit fly) 583 cell-cell signaling [GO:0007267]; establishment of imaginal disc-derived wing hair orientation [GO:0001737]; establishment of ommatidial planar polarity [GO:0042067]; establishment of planar polarity [GO:0001736]; imaginal disc growth [GO:0007446]; imaginal disc-derived leg joint morphogenesis [GO:0016348]; imaginal disc-derived wing vein specification [GO:0007474]; Notch signaling pathway [GO:0007219]; protein phosphorylation [GO:0006468]; regulation of establishment of planar polarity [GO:0090175]; regulation of protein binding [GO:0043393]; regulation of tube length, open tracheal system [GO:0035159]; Wnt signaling pathway [GO:0016055]  cytoplasm [GO:0005737]; extracellular space [GO:0005615]; Golgi membrane [GO:0000139]; plasma membrane [GO:0005886] cytoplasm [GO:0005737]; extracellular space [GO:0005615]; Golgi membrane [GO:0000139]; plasma membrane [GO:0005886]; protein serine kinase activity [GO:0106310]; protein serine/threonine kinase activity [GO:0004674]; Wnt-protein binding [GO:0017147]; cell-cell signaling [GO:0007267]; establishment of imaginal disc-derived wing hair orientation [GO:0001737]; establishment of ommatidial planar polarity [GO:0042067]; establishment of planar polarity [GO:0001736]; imaginal disc growth [GO:0007446]; imaginal disc-derived leg joint morphogenesis [GO:0016348]; imaginal disc-derived wing vein specification [GO:0007474]; Notch signaling pathway [GO:0007219]; protein phosphorylation [GO:0006468]; regulation of establishment of planar polarity [GO:0090175]; regulation of protein binding [GO:0043393]; regulation of tube length, open tracheal system [GO:0035159]; Wnt signaling pathway [GO:0016055]   protein serine kinase activity [GO:0106310]; protein serine/threonine kinase activity [GO:0004674]; Wnt-protein binding [GO:0017147]    GO:0000139; GO:0001736; GO:0001737; GO:0004674; GO:0005615; GO:0005737; GO:0005886; GO:0006468; GO:0007219; GO:0007267; GO:0007446; GO:0007474; GO:0016055; GO:0016348; GO:0017147; GO:0035159; GO:0042067; GO:0043393; GO:0090175; GO:0106310
