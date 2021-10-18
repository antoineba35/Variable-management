import shutil
import ctypes
from tkinter import *
from tkinter import ttk
from tkinter.filedialog import askopenfilename
from functools import partial
import os
import pathlib

couleurText = "#000000"
fondPage = "#FFFFFF"
global tailleTitre, tailleSousTitre, tailleTexte
tailleTitre = 25
tailleSousTitre = 15
tailleTexte = 12
# affichage maximum de 56 variables

global filename, destination
ctypes.windll.user32.MessageBoxW(0, "Choissisez un fichier .py", "Fichier à analyser",  0)
Tk().withdraw() # we don't want a full GUI, so keep the root window from appearing
filename = askopenfilename() # show an "Open" dialog box and return the path to the selected file

destination = os.path.dirname(filename) + "/copie.txt"

class PageVariable:
        
    # titre et bouton retour a l'accueil   
    def titre():
        titre = Label(fenetre, text="Gestion de variables", background=fondPage, font=("Times new roman", tailleTitre, "bold"), fg=couleurText)
        titre.place(x=450, y=10)

    def premierePage():
        nbrValeur = len(tab) 
        global a, x, xxx, y
        x = 15
        a = 1
        y = 75
        xxx = 115
        if(nbrValeur <= 56):
            for cle, valeur in tab.items(): 
                label = Label(fenetre, text=cle, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
                label.place(x=x, y=y)
                bouton = Button(fenetre, text="Modifier "+ cle, width="22", command=partial(
                    PageVariable.vari, cle, valeur
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
        else :
            premCle = [] 
            for cle in tab.keys():
                premCle.append(cle)
            for j in range(0, 56): 
                cle = premCle[j]
                valeurTab = tab.get(cle)
                label = Label(fenetre, text=cle, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
                label.place(x=x, y=y)
                bouton = Button(fenetre, text="Modifier "+ cle, width="22", command=partial(
                    PageVariable.vari, cle, valeurTab
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
            # 2 eme page
            buttonDeuxPage = Button(fenetre, text="->", background="black", fg='white', command=PageVariable.deuxiemePage)
            buttonDeuxPage.place(x=1050, y=20)
    
    # si plus de 54 variables ecrire sur la deuxieme page
    def deuxiemePage():
        if(len(tab) > 128):
            global a, x, xxx, y
            x = 15
            a = 1
            y = 75
            xxx = 115
            for w in fenetre.winfo_children():
                w.destroy()
            PageVariable.titre()
            defAutres.onglets()  
            buttonPremierePage = Button(fenetre, text="<-", background="black", fg='white', command=PageVariable.premierePage)
            buttonPremierePage.place(x=50, y=20)
            buttonTroisiemePage = Button(fenetre, text="->", background="black", fg='white', command=PageVariable.troisiemePage)
            buttonTroisiemePage.place(x=850, y=20)
            premCle = [] 
            
            for cle in tab.keys():
                premCle.append(cle)
            for j in range(56, 128): 
                cle = premCle[j]
                valeurTab = tab.get(cle)
                label = Label(fenetre, text=cle, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
                label.place(x=x, y=y)
                bouton = Button(fenetre, text="Modifier "+ cle, width="22", command=partial(
                    PageVariable.vari, cle, valeurTab
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
        else:
            x = 15
            a = 1
            y = 75
            xxx = 115
            for w in fenetre.winfo_children():
                w.destroy()
            PageVariable.titre()
            defAutres.onglets()  
            buttonPremierePage = Button(fenetre, text="<-", background="black", fg='white', command=PageVariable.premierePage)
            buttonPremierePage.place(x=50, y=20)
            premCle = [] 
            
            for cle in tab.keys():
                premCle.append(cle)
            for j in range(56, len(tab)): 
                cle = premCle[j]
                valeurTab = tab.get(cle)
                label = Label(fenetre, text=cle, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
                label.place(x=x, y=y)
                bouton = Button(fenetre, text="Modifier "+ cle, width="22", command=partial(
                    PageVariable.vari, cle, valeurTab
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
    
    # si plus de 128 variables ecrire sur la troisieme page
    def troisiemePage():
        global a, x, xxx, y
        x = 15
        a = 1
        y = 75
        xxx = 115
        for w in fenetre.winfo_children():
            w.destroy()
        PageVariable.titre()
        defAutres.onglets()  
        buttonPremierePage = Button(fenetre, text="<-", background="black", fg='white', command=PageVariable.deuxiemePage)
        buttonPremierePage.place(x=50, y=20)
        premCle = [] 
        
        for cle in tab.keys():
            premCle.append(cle)
        for j in range(128, len(tab)): 
            cle = premCle[j]
            valeurTab = tab.get(cle)
            label = Label(fenetre, text=cle, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
            label.place(x=x, y=y)
            bouton = Button(fenetre, text="Modifier "+ cle, width="22", command=partial(
                PageVariable.vari, cle, valeurTab
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
                       
    # page d'accueil
    def menuVariable():   
        for w in fenetre.winfo_children():
            w.destroy()
        PageVariable.titre()
        defAutres.onglets()
        
        # retour en arriere
        boutonAccueil = Button(fenetre, text="<-", background="black", fg='white', command=PageAccueil.accueil)
        boutonAccueil.place(x=50, y=10)
        
        # création du tableau des variables
        global tab
        tab = {}
        fichier = open(filename, "r")    
        
        ligne = fichier.readlines()
        for ligne in ligne:
            # vérif si il y a une variable
            if("=" in ligne):
                if('if' in ligne):
                    continue
                variable = ""
                resultat = ""
                position = ligne.index("=")
                # ecriture de la variable
                for i in range(0, position):
                    if(ligne[i] == "+" or ligne[i] == "[" or ligne[i] == "." or ligne[i] == "("):
                        i += 1
                        break            
                    if(ligne[i] != " "):
                        # récupération du nom
                        variable += ligne[i]                    
                longeurs = len(ligne)
                debut = position+1
                for i in range (debut,  longeurs) :
                    if(i != debut):
                        resultat += ligne[i]
                # si la variable n'est pas dans la liste, on l'ajoute et on écrit sur la fenetre
                if(variable not in tab):
                    tab[variable] = resultat
                    
        PageVariable.premierePage()

    # ouverture de la fenetre avec la variable
    def voirPlus(resultat, valeur):
        ctypes.windll.user32.MessageBoxW(0, valeur + " = " + resultat, "Valeur de la variable",  0)

    # voir et modifier la variable      
    def vari(variable, resultat):
        for w in fenetre.winfo_children():
            w.destroy()
        PageVariable.titre() 
        defAutres.onglets() 
        
        # retour en arriere
        boutonAccueil = Button(fenetre, text="<-", background="black", fg='white', command=PageVariable.menuVariable)
        boutonAccueil.place(x=50, y=10)
        
        # info de la variable
        # titre
        voirVariable = Label(fenetre, text="Variables :", background=fondPage, font=("Times new roman", tailleSousTitre, "bold"), fg=couleurText)
        voirVariable.place(x=75, y=65)
        nomVariable = Label(fenetre, text="Nom de la variable : " + variable, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        nomVariable.place(x=25, y=100) 
        # valeur au debut de la variable
        resultatVariable = Label(fenetre, text="Resultat de la variable : " + resultat, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        resultatVariable.place(x=25, y=125)
        # bouton pour ouvir une popup avec les infos de la variables
        boutonpoints = Button(fenetre, background="Grey", width="3", command=partial(
                    PageVariable.voirPlus, resultat, variable
                ))
        boutonpoints.place(x=1000, y=125)
        # analyse et affichage du type de la variable
        typeVar = PageVariable.testType.defTypeVariable(resultat)
        texte = "Type de variable : " + typeVar
        typeVariable = Label(fenetre, text = texte, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        typeVariable.place(x=25, y=150)
        
        # voir la ligne 
        ligne = PageVariable.voirLigne(variable)
        textLigne = "Ligne de la déclaration de la variable : " + str(ligne)
        resultatLigne = Label(fenetre, text=textLigne, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        resultatLigne.place(x=25, y=200)
        
        # voir le nombre de fois que la variable est appeler
        nbrVariableAfficher = Label(fenetre, text="Nombre de fois que la variable est utilisé :", background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        nbrVariableAfficher.place(x=25, y=250)
        nbrFoisAff = defAutres.nbrVariableUtil(variable)
        nbrFoisAffLabel = Label(fenetre, text=nbrFoisAff, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        nbrFoisAffLabel.place(x=280, y=250)
        btnAfficherLigne = Button(fenetre, text="Afficher où est appeler la variable",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=partial(
            PageVariable.afficherOuVariable, variable
        ))
        btnAfficherLigne.place(x=315, y=245)
        
        # modification de la variable
        modifierVariable = Label(fenetre, text="Modifier la variable " + variable, background=fondPage, font=("Times new roman", tailleSousTitre, "bold"), fg=couleurText)
        modifierVariable.place(x=450, y=300)
        
        # modifier nom de la variable
        modifierTitreNomVariable = Label(fenetre, text="Modifier la variable :", background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        modifierTitreNomVariable.place(x=25, y=350)
        global modifierNomVariable 
        modifierNomVariable = Entry(fenetre, background="white", bd=2, font=("Times new roman", tailleTexte))
        modifierNomVariable.insert(END, variable)
        modifierNomVariable.place(x=160, y=350)
        buttonModifierNom = Button(fenetre, text="Modifier nom",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=partial(
            PageVariable.changerNomVariable, variable
        ))
        buttonModifierNom.place(x=350, y=345)

    # fonction chercher ligne de la variable
    def voirLigne(variable):
        fichier = open(filename, "r")    
        i = 1
        ligne = fichier.readlines()
        variable += " ="
        for ligne in ligne:
            # vérif si il y a une variable
            if(variable in ligne):
                if('if' in ligne):
                    continue
                else:
                    return i
            i+=1
            
    # changer le nom de la variable
    def changerNomVariable(variable):
        resultat = modifierNomVariable.get()
        # Read in the file
        with open(destination, 'r') as file :
            filedata = file.read()

        # Replace the target string
        filedata = filedata.replace(variable, resultat)

        # Write the file out again
        with open(destination, 'w') as file:
            file.write(filedata)  
        PageVariable.modifierFichier()   
        PageVariable.menuVariable()
                    
    # class pour les test de types de variables
    class testType :
        # test du type variable
        def defTypeVariable(resultat):
            type = ""
            resultat.replace(" ", "")
            resultat = str(resultat)
            if PageVariable.testType.testBool(resultat) == True and PageVariable.testType.testTableau(resultat) == False:
                type = "Boolean"
            elif PageVariable.testType.testChiffre(resultat) == True:
                type = "Integer" 
            elif PageVariable.testType.testLettre(resultat) == True and PageVariable.testType.testTableau(resultat) == False:
                type = "String"
            elif PageVariable.testType.testFloat(resultat) == True and PageVariable.testType.testTableau(resultat) == False:
                type = "Float"
            elif PageVariable.testType.testTableau(resultat) == True:
                type = "Tableau"
            elif PageVariable.testType.testDictionnaire(resultat) == True:
                type = "Dictionnaire"
            else:
                type = "Pas trouvé" 
            return type

        # si c'est un string
        def testLettre(resultat):
            lettres = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "x", "x", "y", "z", '"']
            if resultat == "True" or resultat == "False":
                return False
            elif resultat == '""':
                return True
            else:
                for i in range(0, len(lettres)):
                    if lettres[i] in resultat:
                        return True
                    elif lettres[i].upper() in resultat:
                        return True
                return False

        # si c'est un int
        def testChiffre(resultat):
            resultat.replace("'", "")
            resultat.replace('"', "")
            if PageVariable.testType.testLettre(resultat):
                return False
            if "." in resultat or "," in resultat:
                return False
            chiffres = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, "+", "-", "*", "/"]
            for i in range(0, len(chiffres)):
                if str(chiffres[i]) in resultat:
                    return True
            return False

        # si c'est un boolean
        def testBool(resultat):
            booleanTest = ["True","False"]
            for i in range(0, len(booleanTest)):
                if str(booleanTest[i]) in resultat:
                    return True
            return False

        # si c'est un float
        def testFloat(resultat):
            tstFloat = [".", ","]
            for i in range(0, len(tstFloat)):
                if str(tstFloat[i]) in resultat:
                    return True
            return False

        # si c'est un tableau
        def testTableau(resultat):
            tableau = ["[", "]"]
            for i in range(0, len(tableau)):
                if str(tableau[i]) in resultat:
                    return True
            return False
        
        def testDictionnaire(resultat):
            dictionnaire = ["{", "}"]
            for i in range(0, len(dictionnaire)):
                if str(dictionnaire[i]) in resultat:
                    return True
            return False
    
    # remodifier le fichier en temps réel 
    def modifierFichier():
        # recopie du fichier
        shutil.copyfile(destination, filename)

    # savoir sur toute les lignes où est situé la variable
    def afficherOuVariable(variable):
        msg = ""
        fichier = open(filename, "r")  
        i = 0
        ligne = fichier.readlines()
        for ligne in ligne: 
            if variable in ligne:
                i+=1
                msg += "\t" + "Ligne"+ str(i) + " = " + str(ligne.lstrip()) + "\n"
        ctypes.windll.user32.MessageBoxW(0, msg, variable,  0, )
                        
    
class PageAccueil:
    def accueil():
        for w in fenetre.winfo_children():
            w.destroy()
        defAutres.onglets()
        titre = Label(fenetre, text="Gestion de variables", background=fondPage, font=("Times new roman", tailleTitre, "bold"), fg=couleurText)
        titre.place(x=400, y=300)

class PageFonction:
    def voirFonction():
        for w in fenetre.winfo_children():
            w.destroy()
        defAutres.onglets()

class PageOption:   
    def option():
        for w in fenetre.winfo_children():
            w.destroy()
        PageOption.titreOption()
        PageOption.optionsChoix()
        
    def titreOption():
        boutonAccueil = Button(fenetre, text="<-", background="black", fg='white', command=PageAccueil.accueil)
        boutonAccueil.place(x=50, y=10)
        titre = Label(fenetre, text="Options", background=fondPage, font=("Times new roman", tailleTitre, "bold"), fg=couleurText)
        titre.place(x=450, y=10)
        defAutres.onglets()
    
    def optionsChoix():
        for w in fenetre.winfo_children():
            w.destroy()
        PageOption.titreOption()
        # modification affichage
        buttonCouleur = Button(fenetre, text="Couleur et option d'affichage", background="#D3D3D3", fg='black', command=PageOption.optionAffichage, width=25)
        buttonCouleur.place(x=50, y=100)
        
        buttonCouleur = Button(fenetre, text="Fichier", background="#D3D3D3", fg='black', command=PageOption.optionFichier, width=25)
        buttonCouleur.place(x=235, y=100)
    
    def optionAffichage():
        for w in fenetre.winfo_children():
            w.destroy()
        PageOption.optionsChoix()
        titre = Label(fenetre, text="Options d'affichage", background=fondPage, font=("Times new roman", tailleTitre, "bold"), fg=couleurText)
        titre.place(x=450, y=10)
        
        # changement taille texte
        global modifTailletexte
        modifTailletexte = Entry(fenetre, background="white", bd=2, font=("Times new roman", tailleTexte))
        modifTailletexte.insert(END, tailleTexte)
        
        global taille
        taille = tailleTexte
        type = "texte"
        
        modifTailletexte.place(x=50, y=150)
        buttonModifierTaille = Button(fenetre, text="Modifier taille du texte",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=partial(
            defAutres.changerTaille,type
        ))
        buttonModifierTaille.place(x=55, y=195)
        
         # changement taille sous-titre
        global modifTailleSousTitre
        
        modifTailleSousTitre = Entry(fenetre, background="white", bd=2, font=("Times new roman", tailleTexte))
        modifTailleSousTitre.insert(END, tailleSousTitre)
        type2 = "sous-titre"
        
        modifTailleSousTitre.place(x=350, y=150)
        buttonModifierTaille = Button(fenetre, text="Modifier taille des sous-titres",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=partial(
            defAutres.changerTaille, type2
        ))
        buttonModifierTaille.place(x=342, y=195)
        
         # changement taille titre
        global modifTailleTitre
        
        modifTailleTitre = Entry(fenetre, background="white", bd=2, font=("Times new roman", tailleTexte))
        modifTailleTitre.insert(END, tailleTitre)
        type3 = "titre"
        
        modifTailleTitre.place(x=650, y=150)
        buttonModifierTaille = Button(fenetre, text="Modifier taille des titres",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=partial(
            defAutres.changerTaille, type3
        ))
        buttonModifierTaille.place(x=655, y=195)
        
        # couleur de fond
        titreFond = Label(fenetre, text="Modifier couleur de fond :", background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        titreFond.place(x=50, y=300)
        
        global comboFond
        comboFond = ttk.Combobox(fenetre, 
                            values=[
                                    "Blanc", 
                                    "Noir"])
        comboFond.place(x=250, y=300)
        buttonModifierFond = Button(fenetre, text="Modifier couleur de fond",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=defAutres.changer_fond)
        buttonModifierFond.place(x=450, y=295)
        
        # changement couleur de texte
        couleurTexte = Label(fenetre, text="Modifier couleur de texte :", background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        couleurTexte.place(x=50, y=350)
        
        global comboCouleurtexte
        comboCouleurtexte = ttk.Combobox(fenetre, values=["Blanc", "Noir", "Rouge", "Vert", "Bleu", "Jaune"])
        comboCouleurtexte.place(x=250, y=350)
        buttonModifierCouleurTexte = Button(fenetre, text="Modifier couleur de texte",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=defAutres.changer_couleurTexte)
        buttonModifierCouleurTexte.place(x=450, y=345)

    def optionFichier():
        for w in fenetre.winfo_children():
            w.destroy()
        PageOption.optionsChoix()
        
        titre = Label(fenetre, text="Options de fichier", background=fondPage, font=("Times new roman", tailleTitre, "bold"), fg=couleurText)
        titre.place(x=450, y=10)
        
        # affichage du dossier en cours
        titreDossierSelectionner = Label(fenetre, text="Fichier sélectionné en cours : ", background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        titreDossierSelectionner.place(x=100, y=150)
        
        dossierSelectionner = Label(fenetre, text=filename, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        dossierSelectionner.place(x=50, y=180)
        
        # changer de dossier
        changerFichier = Button(fenetre, text="Changer de dossier",background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText, command=defAutres.changer_dossier)
        changerFichier.place(x=450, y=210)
        
        # afficher destination
        titreDestination = Label(fenetre, text="Fichier créer automatiquement : ", background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        titreDestination.place(x=100, y=230)
        
        destinationAuto = Label(fenetre, text=destination, background=fondPage, font=("Times new roman", tailleTexte), fg=couleurText)
        destinationAuto.place(x=50, y=260)

class defAutres:
    
    def nbrVariableUtil(variable):
        fichier = open(filename, "r")  
        ligne = fichier.readlines()
        nbr = 0
        for ligne in ligne: 
            if variable in ligne:
                nbr +=1
        return nbr
    
    def changer_dossier():
        global destination, filename
        print("avant :")
        print(filename)
        print(destination) 
        ctypes.windll.user32.MessageBoxW(0, "Choissisez un fichier .py", "Fichier à analyser",  0)
        Tk().withdraw() # we don't want a full GUI, so keep the root window from appearing
        filename = askopenfilename() # show an "Open" dialog box and return the path to the selected file

        print("apres :")
        print(filename)
        print(destination) 
        destination = os.path.dirname(filename) + "/copie.txt"
        PageVariable.menuVariable()   
        
    def changerTaille(typeTexte):
        if typeTexte == "texte":
            size = modifTailletexte.get()
            global tailleTexte
            tailleTexte = size              
            PageOption.optionsChoix()    
            PageOption.optionAffichage()
        elif typeTexte == "sous-titre"  :
            size = modifTailleSousTitre.get()
            global tailleSousTitre
            tailleSousTitre = size
            PageOption.optionsChoix() 
            PageOption.optionAffichage()
        elif typeTexte == "titre"  :
            size = modifTailleTitre.get()
            global tailleTitre
            tailleTitre = size
            PageOption.optionsChoix()
            PageOption.optionAffichage()
                
    def onglets():
        menu = Menu(fenetre)
        
        # ajout menu pour voir les variable
        menu.add_cascade(label="Voir les variables", command=PageVariable.menuVariable)
        menu.add_cascade(label="Voir les fonctions", command=PageFonction.voirFonction)
        menu.add_cascade(label="Options", command=PageOption.option)
        
        fenetre.config(menu=menu)

    def changer_fond():
        global fondPage
        fond = comboFond.get()
        if fond == "Blanc":
            fondPage = "#FFFFFF" 
        elif fond == "Noir":
            fondPage = "#000000"
        fenetre['bg'] = fondPage
        PageOption.optionAffichage()

    def changer_couleurTexte():
        global couleurText
        couleur = comboCouleurtexte.get()
        if couleur == "Blanc":
            couleurText = "#FFFFFF" 
        elif couleur == "Noir":
            couleurText = "#000000"
        elif couleur == "Rouge":
            couleurText = "#f00020"
        elif couleur == "Vert":
            couleurText = "#00561b"
        elif couleur == "Bleu":
            couleurText = "#0080ff"
        elif couleur == "Jaune":
            couleurText = "#ffff00"
        PageOption.optionAffichage()
        
# création de la fenetre
global fenetre
fenetre = Tk()
fenetre.geometry('1100x800')
fenetre.title("Gestionnaire de variable")
fenetre['bg'] = fondPage
fenetre.resizable(height=False, width=False)

extension = pathlib.Path(filename)

# copie du fichier
if(filename != '' and destination !='' and extension.suffix ==".py"):
    shutil.copyfile(filename, destination)
    # création du tableau des variables
    tab = {}
    fichier = open(filename, "r")      

    PageAccueil.accueil()
    defAutres.onglets()

    # faire apparaitre la fenetre à l'infinie
    fenetre.mainloop()

    fichier.close()

    # recopie du fichier
    shutil.copyfile(destination, filename)
    
    os.chdir(os.path.dirname(filename))
    os.remove("copie.txt")
else:
    fenetre.quit()
    if(extension.suffix !=".py"):
        ctypes.windll.user32.MessageBoxW(0, "Désolé ce n'est pas un fichier Python", "Erreur",  0)