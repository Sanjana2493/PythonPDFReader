# PythonPDFReader
#A python code that reads all the PDF files in a directory and finds the number of instances of certain key words - The output is saved on a spreadsheet.
#Works for other file formats as well! 
from tika import parser

import os
path = 'File path in local dict.'

key_words = ['data analytics','big data', 'retail', 'analytics', 'business analytics', 'business intelligence', 'big data analytics', 'blockchain', 'artificial intelligence', 'machine learning', 'data science', 'data scientist', 'IoT']

import xlwt
workBook = xlwt.Workbook()
workSheet = workBook.add_sheet("Name of WorkSheet")

col = 1

workSheet.write(0,0," ")

for word in key_words:
    workSheet.write(0,col,word)
    col += 1
    
import string
p = string.punctuation
d = string.digits
table = str.maketrans(p,len(p)* ' ')
table1 = str.maketrans(d,len(d)* ' ')

import nltk
stopwords = nltk.corpus.stopwords.words("english")

row = 1
row1 = 1

for path,dirs,files in os.walk(path):
    content_str = ''
    for file in files:
        print(file,':')
        company = file.split('.')
        print(company[0])
        workSheet.write(row,0,company[0])
        file_path = os.path.join(path,file)
        parsedPDF = parser.from_file(file_path)
        content = parsedPDF['content']
        content_edit = content.translate(table)
        content_edit1 = content_edit.translate(table1)
        content_edit2 = content_edit1.lower()
        content_words = content_edit2.split()
        col1 = 1
        for word in key_words:
            counter = content_words.count(word)
            workSheet.write(row1,col1,str(counter))
            if(counter > 0):
                print(word , 'is present')
            else:
                print(word , 'is not present')
            col1 += 1
        print()
        row += 1
        row1 += 1

workBook.save("Name of Spreadsheet.xls")
