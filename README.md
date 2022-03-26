# Домашнее задание №3
Код:  
```
!curl -O https://raw.githubusercontent.com/deepjavalibrary/d2l-java/master/tools/fix-colab-gpu.sh && bash fix-colab-gpu.sh
!curl -O https://raw.githubusercontent.com/deepjavalibrary/d2l-java/master/tools/colab_build.sh && bash colab_build.sh
!java --list-modules | grep "jdk.jshell"
!wget http://compbio.mit.edu/ChromHMM/ChromHMM.zip
!unzip /content/ChromHMM.zip

# H3K9ac
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k09acAlnRep1.bam -O H3k09acAlnRep1.bam
# H3K79m2
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k79me2AlnRep1.bam -O H3k79me2AlnRep1.bam
# H3k4me3
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k04me3AlnRep1.bam -O H3k04me3AlnRep1.bam
# H3K4me2
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k04me2AlnRep1.bam -O H3k04me2AlnRep1.bam
# H3K4me1
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k04me1AlnRep1.bam -O H3k04me1AlnRep1.bam
# H3K36me3
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k36me3AlnRep1.bam -O H3k36me3AlnRep1.bam
# H3K27me3
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k27me3AlnRep1.bam -O H3k27me3AlnRep1.bam
# H3K27ac
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k27acAlnRep1.bam -O H3k27acAlnRep1.bam
# H2AFZ
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H2azAlnRep1.bam -O H2azAlnRep1.bam
# Control
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41ControlStdAlnRep1.bam -O ControlStdAlnRep1.bam
# H3K9me3
!wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/encodeDCC/wgEncodeBroadHistone/wgEncodeBroadHistoneDnd41H3k09me3AlnRep1.bam -O H3k09me3AlnRep1.bam

!java -mx5000M -jar /content/ChromHMM/ChromHMM.jar BinarizeBam -b 200  /content/ChromHMM/CHROMSIZES/hg19.txt /content/ cellmarkfiletable.txt   binarizedData

!java -mx5000M -jar /content/ChromHMM/ChromHMM.jar LearnModel /content/binarizedData result 10 hg19

!zip -r result.zip result

from google.colab import files
files.download("result.zip")

# Часть 2

skipped = 0
new_names = {'1' : '1_Repressed', '2' : '2_Heterochromatin', '3' : '3_Repressed', '4' : '4_Transcribed', '5' : '5_Transcribed', '6' : '6_Transcribed', '7' : '7_Transcribed', '8' : '8_Repressed', '9' : '9_Repressed', '10' : '10_Active_Promoter'}

with open('Dnd41_10_dense.bed') as file:
  with open('Dnd41_10_modified_dense.bed','w') as f:
    for line in file:
      if skipped == 0:
        f.write(line)
        skipped = 1
      else:
        data = line
        data = data.split('\t')
        data[3] = new_names[data[3]]
        f.write('\t'.join(data))
        
!head Dnd41_10_modified_dense.bed
```
## Часть 1.
### 1. Таблицы
|Метки   |Файлы              |
|:------:|:-----------------:|
|H3K9ac  |H3k09acAlnRep1.bam |
|H3K79me2|H3k79me2AlnRep1.bam|
|H3k4me3 |H3k04me3AlnRep1.bam|
|H3K4me2 |H3k04me2AlnRep1.bam|
|H3K4me1 |H3k04me1AlnRep1.bam|
|H3K36me3|H3k36me3AlnRep1.bam|
|H3K27me3|H3k27me3AlnRep1.bam|
|H3K27ac |H3k27acAlnRep1.bam |
|H2AFZ   |H2azAlnRep1.bam    |
|H3K9me3 |H3k09me3AlnRep1.bam|

|Состояние|Метки|Типичное расположение относительно CpG островков|Типичное расположение относительно аннотированных геномов|Типичное расположение относительно LAD'ов|Изображение|Названия|
|--|--|--|--|--|--|--|
|1|H3K27me3|Не зависит|Вне гена|Обычно на ней|![image](https://user-images.githubusercontent.com/55440084/160194977-e12eb0a5-6619-4eb8-bbac-83c48dc1fc8f.png)|Repressed|
|2|-|Не зависит|Вне гена|Обычно на ней|![image](https://user-images.githubusercontent.com/55440084/160196273-3ecb4401-18d9-4316-b186-171bfad1d692.png)|Heterochromatin|
|3|-|Не зависит|Вне гена|Обычно на ней|![image](https://user-images.githubusercontent.com/55440084/160196280-6129346d-86b0-4ccb-a32a-9baa24710e7d.png)|Repressed|
|4|H3K36me3|Не зависит|На изображении не видно, но часто конец транскрипции|Вне ее|![image](https://user-images.githubusercontent.com/55440084/160194789-46c66cb9-9652-450e-8067-97da07723bbb.png)|Transcribed|
|5|H3K36me3 H3K79me2|Не зависит|Экзоны|Вне ее|![image](https://user-images.githubusercontent.com/55440084/160196105-18b9d645-1663-4236-ade1-a557d5d43b07.png)|Transcribed|
|6|H3K79me2|Не зависит|Только в гене|Вне ее|![image](https://user-images.githubusercontent.com/55440084/160195305-d0e91e36-a96b-4d1f-82c5-5d277be743fe.png)|Transcribed|
|7|H3K4me1 H3K79me2|Не зависит|Иногда конец транскрипции|Вне ее|![image](https://user-images.githubusercontent.com/55440084/160249651-573c7dd9-210e-40e9-a11f-80bc0fc1eff3.png)|Transcribed|
|8|H3K4me1|Не зависит|Везде|Вне ее|![image](https://user-images.githubusercontent.com/55440084/160194513-ab801ef8-f7eb-48a6-9adb-3a49207abbe2.png)|Repressed|
|9|H3K4me1|Не зависит|Везде|Вне ее|![image](https://user-images.githubusercontent.com/55440084/160194506-a9fe55ae-b8db-4e71-85a8-23cc164015f3.png)|Repressed|
|10|H2AFZ H3K4me2 H3k4me3 H3K9ac H3K27ac H3K79me2|Совпадает|Начало транскрипции, экзоны|Вне ее|![image](https://user-images.githubusercontent.com/55440084/160192463-bf14885e-10c9-41e5-97f4-0991d0b270e4.png)|Active Promoter|

### 2. Полученные изображения
![image](https://user-images.githubusercontent.com/55440084/159134613-d67cd118-20f7-4757-8408-fdaf9b04e1c1.png)
![image](https://user-images.githubusercontent.com/55440084/159134632-e4806878-c1e0-4333-b76a-e0ba3178f6ad.png)
![image](https://user-images.githubusercontent.com/55440084/159134634-1e96dfbe-9b1a-4969-baac-b0c5da9390da.png)
![image](https://user-images.githubusercontent.com/55440084/159134617-510223c2-cd64-4e82-a587-022d1fec43c2.png)
![image](https://user-images.githubusercontent.com/55440084/159134624-47a24c67-be57-4c02-baf7-8ec789663092.png)

## Часть 2.
Код второй части также лежит в одном файле, вместе с первой. Модифицированный `.bed` файл лежит в репозитории. Скриншот:  
![image](https://user-images.githubusercontent.com/55440084/160251623-7425bb3c-b068-46fc-966e-83231baf5fb4.png)
