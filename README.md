# hangman
a function that helps you to play hangman with computer
import random

WORDLIST_FILENAME = "words.txt"

def loadWords():
    """
    Returns a list of valid words. Words are strings of lowercase letters.
    
    Depending on the size of the word list, this function may
    take a while to finish.
    """
    print("Loading word list from file...")
    # inFile: file
    inFile = open(WORDLIST_FILENAME, 'r')
    # line: string
    line = inFile.readline()
    # wordlist: list of strings
    wordlist = line.split()
    print("  ", len(wordlist), "words loaded.")
    return wordlist

def chooseWord(wordlist):
    """
    wordlist (list): list of words (strings)

    Returns a word from wordlist at random
    """
    return random.choice(wordlist)

# end of helper code
# -----------------------------------

# Load the list of words into the variable wordlist
# so that it can be accessed from anywhere in the program
wordlist = loadWords()

def isWordGuessed(secretWord, lettersGuessed):
    '''
    secretWord: string, the word the user is guessing
    lettersGuessed: list, what letters have been guessed so far
    returns: boolean, True if all the letters of secretWord are in lettersGuessed;
      False otherwise
    '''
    for char in secretWord:
        if char not in lettersGuessed:
            return False
    return True



def getGuessedWord(secretWord, lettersGuessed):
    '''
    secretWord: string, the word the user is guessing
    lettersGuessed: list, what letters have been guessed so far
    returns: string, comprised of letters and underscores that represents
      what letters in secretWord have been guessed so far.
    '''
    ans = ''
    for char in secretWord:
        if char in lettersGuessed:
            ans = ans + char
        else:
            ans = ans + '_'
            ans = ans + ' '
    return ans



def getAvailableLetters(lettersGuessed):
    '''
    lettersGuessed: list, what letters have been guessed so far
    returns: string, comprised of letters that represents what letters have not
      yet been guessed.
    '''
    import string
    earlyList = string.ascii_lowercase
    ans = ''
    for char in earlyList:
        if char not in lettersGuessed:
            ans = ans + char
    return ans




def hangman(secretWord):
    '''
    secretWord: string, the secret word to guess.

    Starts up an interactive game of Hangman.

    * At the start of the game, let the user know how many 
      letters the secretWord contains.

    * Ask the user to supply one guess (i.e. letter) per round.

    * The user should receive feedback immediately after each guess 
      about whether their guess appears in the computers word.

    * After each round, you should also display to the user the 
      partially guessed word so far, as well as letters that the 
      user has not yet guessed.

    Follows the other limitations detailed in the problem write-up.
    '''
    print('Welcome to the game, Hangman')
    print ('I am thinking of a word that is ' + str(len(secretWord)) + ' letters long')
    count = 0
    lettersGuessed = []
    while isWordGuessed(secretWord, lettersGuessed) != True:
        print('You have ' + str(8-count) + ' guesses left.')
        print('Available letters: ' + getAvailableLetters(lettersGuessed))
        guess = input('Please guess a letter: ')
        if guess in lettersGuessed:
            print("Oops! You've already guessed that letter: ")
        else:
            lettersGuessed.append(guess)
            if guess not in secretWord:
                count = count+1
            if getGuessedWord(secretWord,lettersGuessed) == secretWord:
                print('Congragulations, you won!')
                break
            if count >= 8:
                print('Sorry, you ran out of guesses. The word was ' + secretWord)
                break
            if guess in secretWord:
                print('Good guess: ' + getGuessedWord(secretWord, lettersGuessed))
            else:
                print('Oops! That letter is not in my word: ' + getGuessedWord(secretWord, lettersGuessed))
     
        



# When you've completed your hangman function, uncomment these two lines
# and run this file to test!(in the below I already uncommented those two lines) (hint: you might want to pick your own
# secretWord while you're testing)

secretWord = chooseWord(wordlist).lower()
hangman(secretWord)
