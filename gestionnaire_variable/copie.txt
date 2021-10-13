import shutil
import ctypes
from tkinter import *
from tkinter.filedialog import askopenfilename
from functools import partial
import os


ctypes.windll.user32.MessageBoxW(0, "Choissisez le fichier a analyser", "Fichier à analyser",  1)
Tk().withdraw() # we don't want a full GUI, so keep the root window from appearing
filename = askopenfilename() # show an "Open" dialog box and return the path to the selected file

destination = os.path.dirname(filename) + "/copie.txt"

class PageVariable:
        
    # titre et bouton retour a l'accueil   
    def titre():
        boutonAccueil = Button(fenetre, text="<-", background="black", fg='white', command=PageVariable.menuVariable)
        boutonAccueil.place(x=50, y=10)
        titre = Label(fenetre, text="Gestion de variables", background="white", font=("Times new roman", 15, "bold"))
        titre.place(x=450, y=10)

    # page d'accueil
    def menuVariable():   
        x = 15
        a = 1
        y = 75
        xxx = 115
        for w in fenetre.winfo_children():
            w.destroy()
        PageVariable.titre()
        onglets()
        
        # création du tableau des variables
        tab = {}
        fichier = open(filename, "r")    
        
        lignes = fichier.readlines()
        for lignes in lignes:
            # vérif si il y a une variable
            if("=" in lignes):
                if('if' in lignes):
                    continue
                variable = ""
                resultats = ""
                position = lignes.index("=")
                # ecriture de la variable
                for i in range(0, position):
                    if(lignes[i] == "+" or lignes[i] == "[" or lignes[i] == "." or lignes[i] == "("):
                        i += 1
                        break            
                    if(lignes[i] != " "):
                        # récupération du nom
                        variable += lignes[i]
                longeur = len(lignes)
                debut = position+1
                for i in range (debut,  longeur) :
                    if(i != debut):
                        resultats += lignes[i]
                textPrint = variable
                # si la variable n'est pas dans la liste, on l'ajoute et on écrit sur la fenetre
                if(variable not in tab):
                    tab[variable] = resultats
                    label = Label(fenetre, text=variable, background="white", font=("Times new roman", 12))
                    label.place(x=x, y=y)
                    bouton = Button(fenetre, text="Modifier "+variable, width="20", command=partial(
                        PageVariable.vari, textPrint, resultats
                    ))
                    bouton.place(x=xxx, y=y)
                    if(a == 1 or a == 2 or a==3):
                        x += 270
                        xxx += 270
                        a += 1
                    elif(a == 4):
                        x = 15
                        a = 1
                        y += 50
                        xxx = 115            

    # ouverture de la fenetre avec la variable
    def voirPlus(resultats, valeur):
        ctypes.windll.user32.MessageBoxW(0, valeur + " = " + resultats, "Valeur de la variable",  1)

    # voir et modifier la variable      
    def vari(variable, resultats):
        for w in fenetre.winfo_children():
            w.destroy()
        PageVariable.titre()  
        
        # info de la variable
        # titre
        voirVariable = Label(fenetre, text="Variables :", background="white", font=("Times new roman", 15, "bold"))
        voirVariable.place(x=75, y=65)
        # nom de la variable
        nomVariable = Label(fenetre, text="Nom de la variable : " + variable, background="white", font=("Times new roman", 12))
        nomVariable.place(x=25, y=100) 
        # valeur au debut de la variable
        resultatsVariable = Label(fenetre, text="Resultat de la variable : " + resultats, background="white", font=("Times new roman", 12))
        resultatsVariable.place(x=25, y=125)
        # bouton pour ouvir une popup avec les infos de la variables
        boutonpoints = Button(fenetre, background="Grey", width="3", command=partial(
                    PageVariable.voirPlus, resultats, variable
                ))
        boutonpoints.place(x=1000, y=125)
        # analyse et affichage du type de la variable
        typeVar = PageVariable.testType.defTypeVariable(resultats)
        texte = "Type de variable : " + typeVar
        typeVariable = Label(fenetre, text = texte, background="white", font=("Times new roman", 12))
        typeVariable.place(x=25, y=150)
        
        # voir la ligne 
        ligne = PageVariable.voirLigne(variable)
        textLigne = "Ligne de la déclaration de la variable : " + str(ligne)
        resultatLigne = Label(fenetre, text=textLigne, background="white", font=("Times new roman", 12))
        resultatLigne.place(x=25, y=200)
        
        # modification de la variable
        modifierVariable = Label(fenetre, text="Modifier la variable " + variable, background="white", font=("Times new roman", 15, "bold"))
        modifierVariable.place(x=450, y=250)
        
        # modifier nom de la variable
        modifierTitreNomVariable = Label(fenetre, text="Modifier la variable :", background="white", font=("Times new roman", 12))
        modifierTitreNomVariable.place(x=25, y=300)
        global modifierNomVariable 
        modifierNomVariable = Entry(fenetre, background="white", bd=2, font=("Times new roman", 12))
        modifierNomVariable.insert(END, variable)
        modifierNomVariable.place(x=160, y=300)
        buttonModifierNom = Button(fenetre, text="Modifier nom", background="white", font=("Times new roman", 12), command=partial(
            PageVariable.changerNomVariable, variable
        ))
        buttonModifierNom.place(x=350, y=295)

    # fonction chercher ligne de la variable
    def voirLigne(variable):
        fichier = open(filename, "r")    
        i = 1
        lignes = fichier.readlines()
        variable += " ="
        for lignes in lignes:
            # vérif si il y a une variable
            if(variable in lignes):
                if('if' in lignes):
                    continue
                else:
                    return i
            i+=1
            
    # changer le nom de la variable
    def changerNomVariable(variable):
        resultats = modifierNomVariable.get()
        # Read in the file
        with open(destination, 'r') as file :
            filedata = file.read()

        # Replace the target string
        filedata = filedata.replace(variable, resultats)

        # Write the file out again
        with open(destination, 'w') as file:
            file.write(filedata)  
        PageVariable.modifierFichier()   
        PageVariable.menuVariable()
                    
    # class pour les test de types de variables
    class testType :
        # test du type variable
        def defTypeVariable(resultats):
            type = ""
            resultats.replace(" ", "")
            resultats = str(resultats)
            if PageVariable.testType.testBool(resultats) == True and PageVariable.testType.testTableau(resultats) == False:
                type = "Boolean"
            elif PageVariable.testType.testChiffre(resultats) == True:
                type = "Integer" 
            elif PageVariable.testType.testLettre(resultats) == True and PageVariable.testType.testTableau(resultats) == False:
                type = "String"
            elif PageVariable.testType.testFloat(resultats) == True and PageVariable.testType.testTableau(resultats) == False:
                type = "Float"
            elif PageVariable.testType.testTableau(resultats) == True:
                type = "Tableau"
            elif PageVariable.testType.testDictionnaire(resultats) == True:
                type = "Dictionnaire"
            else:
                type = "Pas trouvé" 
            return type

        # si c'est un string
        def testLettre(resultats):
            lettres = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "x", "x", "y", "z", '""']
            if resultats == "True" or resultats == "False":
                return False
            elif resultats == '""':
                return True
            else:
                for i in range(0, len(lettres)):
                    if lettres[i] in resultats:
                        return True
                    elif lettres[i].upper() in resultats:
                        return True
                return False

        # si c'est un int
        def testChiffre(resultats):
            resultats.replace("'", "")
            resultats.replace('"', "")
            if PageVariable.testType.testLettre(resultats):
                return False
            if "." in resultats or "," in resultats:
                return False
            chiffres = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, "+", "-", "*", "/"]
            for i in range(0, len(chiffres)):
                if str(chiffres[i]) in resultats:
                    return True
            return False

        # si c'est un boolean
        def testBool(resultats):
            booleanTest = ["True","False"]
            for i in range(0, len(booleanTest)):
                if str(booleanTest[i]) in resultats:
                    return True
            return False

        # si c'est un float
        def testFloat(resultats):
            tstFloat = [".", ","]
            for i in range(0, len(tstFloat)):
                if str(tstFloat[i]) in resultats:
                    return True
            return False

        # si c'est un tableau
        def testTableau(resultats):
            tableau = ["[", "]"]
            for i in range(0, len(tableau)):
                if str(tableau[i]) in resultats:
                    return True
            return False
        
        def testDictionnaire(resultats):
            dictionnaire = ["{", "}"]
            for i in range(0, len(dictionnaire)):
                if str(dictionnaire[i]) in resultats:
                    return True
            return False
        
    def modifierFichier():
        # recopie du fichier
        shutil.copyfile(destination, filename)

class PageAccueil:
    def accueil():
        titre = Label(fenetre, text="Gestion de variables", background="white", font=("Times new roman", 50, "bold"))
        titre.place(x=300, y=300)

def onglets():
    menu = Menu(fenetre)
    
    # ajout menu pour voir les variable
    menu.add_cascade(label="Voir les variables", command=PageVariable.menuVariable)
    
    fenetre.config(menu=menu)
    
# création de la fenetre
fenetre = Tk()
fenetre.geometry('1100x800')
fenetre.title("Gestionnaire de variable")
fenetre['bg'] = "white"
fenetre.resizable(height=False, width=False)

# copie du fichier
shutil.copyfile(filename, destination)

# création du tableau des variables
tab = {}
fichier = open(filename, "r")      

PageAccueil.accueil()
onglets()

# faire apparaitre la fenetre à l'infinie
fenetre.mainloop()

fichier.close()

# recopie du fichier
shutil.copyfile(destination, filename)