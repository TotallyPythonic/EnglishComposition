import os
import urllib.request
from random import randint
from bs4 import BeautifulSoup

""" variables that will store the frequency of the used websites """

c_etymonline = 0
c_memidex = 0

""" the lists that will be used to store the absolute frequency of the sorted words """

old_english_absolute = []
germanic_absolute = []
french_absolute = []
latin_absolute = []
greek_absolute = []
other_absolute = []

""" the lists that will be used to store the relative frequency of the sorted words """

old_english_rel = []
germanic_rel = []
french_rel = []
latin_rel = []
greek_rel = []
other_rel = []

""" the lists that will be used to store the sorted words themselves """

not_processed = []
list_greek = []
list_germanic = []
list_french = []
list_old_english = []
list_latin = []
list_other = []

""" reading the .txt file with the frequency list """

english_doc = open("english_frequency.txt", "r+", encoding="utf8")
english_data = english_doc.readlines()

""" reading the .txt file line by line, adding the word on every line as an element to english_processed """

english_processed = []
for index, line in enumerate(english_data):
    word = ""
    for char in english_data[index]:
        if char.isalpha() or char == "'":
            word = word + char
            word = word.lower()
    english_processed.append(word)           

""" this function scrapes the data and then tries to interpret in the following order:
1) etymonline (extremely accurate as it was manually compiled by linguists)
2) memidex (very accurate as it is an index of 5-10 dictionaries but has a bias towards Anglo-Saxon as it does not "dig" deep enough for the true origin)
"""

def scrape_and_interpret(term):

    """ etymonline """
    
    try:
        global c_etymonline
        url = urllib.request.Request("http://www.etymonline.com/index.php?term=" + term + "&allowed_in_frame=0")
        etymology_request = urllib.request.urlopen(url)
        etymology = BeautifulSoup(etymology_request, "html.parser")
        etymology_processed = etymology.find("dd").getText().lower()

        word_list = []
        word_list2 = []
        space_count = 0
        temp_word = ""

        """ adds every word as an element to the list word_list for easy interpretation later """
        
        for char in etymology_processed:
            if char == " ":
                word_list.append(temp_word)
                temp_word = ""
            elif char.isalpha():
                temp_word = temp_word + char

        """ because etymonline mentions the correct language of origin right away, we take the first mentioned language; if no language is mentioned after 15 words we stop searching this source"""

        count = 15
        for word in word_list:
            if word.find("greek") > -1 or word.find("latin") > -1 or word.find("french") > -1 or word.find("norman") > -1 or word.find("english") > -1 or word.find("saxon") > -1 or word.find("germanic") > -1 or word.find("dutch") > -1 or word.find("norse") > -1 or word.find("scandinavian") > -1 or word.find("norwegian") > -1 or word.find("danish") > -1 or word.find("icelandic") > -1 or word.find("swedish") > -1:
                if count != 0:
                    word_list2.append(word)
                    count = 0
                elif count > 0:
                    word_list2.append(word)
                    count -= 1

        for word in word_list2:
            if word.find("greek") > -1:
                c_etymonline += 1
                return "greek"
            elif word.find("latin") > -1:
                c_etymonline += 1
                return "latin"
            elif word.find("germanic") > -1 or word.find("dutch") > -1 or word.find("icelandic") > -1 or word.find("norse") > -1 or word.find("norwegian") > -1 or word.find("danish") > -1 or word.find("swedish") > -1 or word.find("german") > -1 or word.find("flemish") > -1 or word.find("norwegian") > -1:
                c_etymonline += 1
                return "germanic"
            elif word.find("english") > -1 or word.find("anglish") > -1 or word.find("saxon") > -1:
                c_etymonline += 1
                return "old_english"
            elif word.find("french") > -1 or word.find("norman") > -1:
                c_etymonline += 1
                return "french"

    except:
        """ if etymonline does not provide an answer, we try memidex  """

    """ memidex """
    
    try:
        global c_memidex
        url = urllib.request.Request("http://www.memidex.com/" + term)
        etymology_request = urllib.request.urlopen(url)
        etymology = BeautifulSoup(etymology_request, "html.parser")
        etymology_processed = etymology.find("div", {"id" : "etymology"}).getText().lower()

        word_list = []
        space_count = 0
        temp_word = ""

        """ again seperates the words and stores them in word_list """

        for char in etymology_processed:
            if char == " ":
                word_list.append(temp_word)
                temp_word = ""
            elif char.isalpha():
                temp_word = temp_word + char
                
        c_greek = 0
        c_latin = 0
        c_germanic = 0
        c_old_english = 0
        c_french = 0

        """ memidex mentions the word "origin" immediately before mentioning the language of origin, to ensure we only receive the first mentioned language, we only retrieve the first 8 words after each source mentions "origin" """
        
        word_list2 = []
        count = 0
        for index, word in enumerate(word_list):
            if word.find("origin") > -1:
                count = 8
            elif count > 0:
                word_list2.append(word)
                count -= 1
            
        """ next, we take the language that was mentioned most often by all sources and return it. In the extremely unlikely event that two or more languages are mentioned equally, we randomly select one of them. """

        for word in word_list2:
            if word.find("greek") > -1:
                c_greek += 1
            elif word.find("latin") > -1:
                c_latin += 1
            elif word.find("germanic") > -1 or word.find("dutch") > -1 or word.find("icelandic") > -1 or word.find("norse") > -1 or word.find("norwegian") > -1 or word.find("danish") > -1 or word.find("swedish") > -1 or word.find("german") > -1 or word.find("flemish") > -1:
                c_germanic += 1
            elif word.find("english") > -1 or word.find("anglish") > -1 or word.find("saxon") > -1:
                c_old_english += 1
            elif word.find("french") > -1 or word.find("norman") > -1:
                c_french += 1
                
        dic = {"greek" : c_greek, "latin" : c_latin, "germanic" : c_germanic, "old_english" : c_old_english, "french" : c_french}
        list_largest = []
        highest = 0
        for value in dic:
            if dic[value] == highest and dic[value] != 0:
                list_largest.append(value)
            if dic[value] > highest and dic[value] != 0:
                del list_largest[:]
                list_largest.append(value)
                highest = dic[value]
        if len(list_largest) == 1:
            c_memidex += 1
            return list_largest[0]
        elif len(list_largest) > 1:
            c_memidex += 1
            return list_largest[randint(0,len(list_largest)-1)]

    except:
        """ if none of these website have results or if we could not process the word (due to it being an extreme variant of the root), we add it to the "other" category """
        return "other"
    
    list_other.append(term)

""" the execution """

def execute(amount):
    
    words = []
    words1 = []

    """ just a little feature to limit the amount of words to process """
    
    for value in english_processed[0:amount]:
        words.append(value)

    """ adverbs are the only words in frequency lists that return false Old English positives for words of French/Latin origin. We drop the "ly" for every word over 5 characters to interpret the root."""
    
    for word in words:
        if word[(len(word)-2):] == "ly" and len(word) > 5:
            words1.append(word.replace("ly", ""))
        else:
            words1.append(word)
            
    count_germanic = 0
    count_latin = 0
    count_french = 0
    count_greek = 0
    count_old_english = 0
    count_other = 0
    
    for word in words1:
        print(word)
        origin = scrape_and_interpret(word)
        if origin == "french":
            count_french += 1
            list_french.append(word)
        elif origin == "latin":
            count_latin += 1
            list_latin.append(word)
        elif origin == "old_english":
            count_old_english += 1
            list_old_english.append(word)
        elif origin == "germanic":
            count_germanic += 1
            list_germanic.append(word)
        elif origin == "greek":
            count_greek += 1
            list_greek.append(word)
        elif origin == "other":
            list_other.append(word)
            count_other += 1

        tot = count_germanic + count_latin + count_french + count_greek + count_old_english + count_other
        
        french_absolute.append(count_french)
        latin_absolute.append(count_latin)
        old_english_absolute.append(count_old_english)
        germanic_absolute.append(count_germanic)
        greek_absolute.append(count_greek)
        other_absolute.append(count_other)

        french_rel.append(count_french / tot)
        latin_rel.append(count_latin / tot)
        old_english_rel.append(count_old_english / tot)
        germanic_rel.append(count_germanic / tot)
        greek_rel.append(count_greek / tot)
        other_rel.append(count_other / tot)

        print("processed: ",(words.index(word) / len(words))*100)
        
    print("\nAbsolute frequency of languages of origin:")
    print("Germanic: %d, Latin: %d, French: %d, Old English: %d, Greek: %d, Other: %d \n" % (count_germanic, count_latin, count_french, count_old_english, count_greek, count_other))
    print("Relative frequency of languages of origin")
    print("Germanic: %f, Latin %f, French %f,  Old English %f, Greek %f, Other: %f \n" % (germanic_rel[len(germanic_rel)-1], latin_rel[len(latin_rel)-1], french_rel[len(french_rel)-1], old_english_rel[len(old_english_rel)-1], greek_rel[len(greek_rel)-1], other_rel[len(other_rel)-1]))
    print("Total processed:", tot)
    print("\n" + "The frequency of the sources:")
    print("etymonline: %f, memidex: %f" % (c_etymonline, c_memidex))

execute(5000)

english_doc.close()
