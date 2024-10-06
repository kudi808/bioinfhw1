# bioinfhw1
ссылки и случайная пятерка
ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
seqtk sample -s629 oil_R1.fastq 5000000 > S1.fastq
seqtk sample -s629 oil_R2.fastq 5000000 > S2.fastq
seqtk sample -s629 oilMP_S4_L001_R1_001.fastq 1500000 > M1.fastq
seqtk sample -s629 oilMP_S4_L001_R2_001.fastq 1500000 > M2.fastq
оцениваем качество чтений

подрезаем чтения с помощью platanus

оцениваем качество подрезанных и добавляем отчет

переходим в screen и делаем сборку контиг

то же с скаффолдами

скачиваем полученные файлы черз scp



