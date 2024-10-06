# bioinfhw1
ссылки и случайная пятерка
```
ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
seqtk sample -s239 oil_R1.fastq 5000000 > S1.fastq
seqtk sample -s239 oil_R2.fastq 5000000 > S2.fastq
seqtk sample -s239 oilMP_S4_L001_R1_001.fastq 1500000 > M1.fastq
seqtk sample -s239 oilMP_S4_L001_R2_001.fastq 1500000 > M2.fastq
```
оцениваем качество чтений
```
mkdir fastqc
mkdir multiqc
ls S* M* | xargs -tI{} fastqc -o fastqc {}
multiqc -o multiqc fastqc
```
подрезаем чтения с помощью platanus
```
platanus_trim S*
platanus_internal_trim M*
```
оцениваем качество подрезанных и добавляем отчет
```
mkdir fastqc_trimmed
mkdir multiqc_trimmed
ls S* M*| xargs -tI{} fastqc -o fastqc_trimmed {}
multiqc -o multiqc_trimmed fastqc_trimmed
```
переходим в screen и делаем сборку контиг
```
screen
time platanus assemble -o Poil -t 4 -f S1.fastq.trimmed S2.fastq.trimmed 2> assemble.log
```
то же с скаффолдами
```
screen
time platanus scaffold -o Poil -t 4 -c Poil_contig.fa -IP1 S1.fastq.trimmed S2.fastq.trimmed -OP2 M1.fastq.int_trimmed M2.fastq.int_trimmed 2> scaffold.log
```
уменьшаем число промежутков
```
screen
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 S1.fastq.trimmed S2.fastq.trimmed -OP2 M1.fastq.int_trimmed M2.fastq.int_trimmed 2> gapclose.log
```
скачиваем полученные файлы черз scp
```
screen
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 S1.fastq.trimmed S2.fastq.trimmed -OP2 M1.fastq.int_trimmed M2.fastq.int_trimmed 2> gapclose.log
```
самый длинный и красивый скаффолд
```
more Poil_gapClosed.fa
echo scaffold1_cov231 > _tmp.txt
seqtk subseq Poil_gapClosed.fa _tmp.txt > longest.fasta
```
Количество гэпов для самого длинного скаффолда до подрезания
```
!grep 'N' scaffolds.fasta | wc -l
```
Количество гэпов для самого длинного скаффолда после подрезания
```
!grep 'N' after_gap.fa | wc -l
```



