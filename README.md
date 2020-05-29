# Tesseract-OCR Fine Tuning on Text Line Images for Bengali

## Commands

mkdir tesseract_fineTune
cd tesseract_fineTune
mkdir data

## Dataset preparation

Choose a model name

git clone https://github.com/tesseract-ocr/tesstrain.git
cd tesstrain
make help
mkdir data 
cd data 
mkdir MODEL_NAME-ground-truth

Place ground truth consisting of line images and transcriptions in the folder data/MODEL_NAME-ground-truth
-- images with extension .tiff or .png or .bin.png or .nrm.png
-- transcriptions with extension .gt.txt
-- example dataset can be found [here](https://github.com/tesseract-ocr/tesstrain/blob/master/ocrd-testset.zip) 

make training MODEL_NAME=name of the resulting model

It will generate a traineddata, box files, lstmf files and a list of all lstmf files. We will need the list and all lstmf files for fine-tuning.

For more information follow [this readme](https://github.com/tesseract-ocr/tesstrain)

## Fine-Tuning 

git clone https://github.com/tesseract-ocr/tesseract.git
git clone https://github.com/tesseract-ocr/tessdata_best.git

The idea of fine tuning is really to apply it to one of the fully-trained existing models.
So copy the traineddata which you want to fine-tune from tessdata_best to tesseract/tessdata. I have used ben.traineddata.
Next generate the lstm file from tarineddata.

mkdir impact_from_full
combine_tessdata -e tesseract/tessdata/ben.traineddata \
  impact_from_full/ben.lstm

Now copy the lstmf files generated earlier to tesseract_fineTune/data and all-lstmf file to tesseract_fineTune.

lstmtraining --model_output impact_from_full/impact \
--continue_from impact_from_full/ben.lstm \
--traineddata tesseract/tessdata/ben.traineddata \
--train_listfile all-lstmf --max_iterations 400
--model_output /path/to/output# tesseract-fine-tuning

# References-
1 https://github.com/tesseract-ocr/tesstrain
2 https://tesseract-ocr.github.io/tessdoc/TrainingTesseract-4.00---Finetune
3 https://tesseract-ocr.github.io/tessdoc/TrainingTesseract-4.00.html#fine-tuning-for-impact
4 https://github.com/tesseract-ocr/tesseract
5 https://github.com/tesseract-ocr/tessdata_best