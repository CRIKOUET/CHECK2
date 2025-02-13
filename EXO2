Function Random-Password ($length = 8)
{
    $punc = 46..46
    $digits = 48..57
    $letters = 65..90 + 97..122

    $password = get-random -count $length -input ($punc + $digits + $letters) |`
        ForEach -begin { $aa = $null } -process {$aa += [char]$_} -end {$aa}
    Return $password.ToString()
}

Function ManageAccentsAndCapitalLetters
{
    param ([String]$String)
    
    $StringWithoutAccent = $String -replace '[éèêë]', 'e' -replace '[àâä]', 'a' -replace '[îï]', 'i' -replace '[ôö]', 'o' -replace '[ùûü]', 'u'
    $StringWithoutAccentAndCapitalLetters = $StringWithoutAccent.ToLower()
    $StringWithoutAccentAndCapitalLetters
}

$Path = "C:\Scripts"
$CsvFile = "$Path\Users.csv"
$LogFile = "$Path\Log.log"

# Q.2.3 modifier le -skip2 en skip1 pour que le permier utilisateur soit pris en compte
# Q.2.5 les differents champs pris en compte sont prenom;nom;fonction et descritption
$Users = Import-Csv -Path $CsvFile -Delimiter ";" -Header "prenom","nom","fonction","description" -Encoding UTF8  | Select-Object -Skip 1

foreach ($User in $Users)
{
    $Prenom = ManageAccentsAndCapitalLetters -String $User.prenom
    $Nom = ManageAccentsAndCapitalLetters -String $User.Nom
    $Name = "$Prenom.$Nom"
    If (-not(Get-LocalUser -Name "$Prenom.$Nom" -ErrorAction SilentlyContinue))
    {
        $Pass = Random-Password
        $Password = (ConvertTo-secureString $Pass -AsPlainText -Force)
        $Description = "$($user.description) - $($User.fonction)"
        # Q.2.4 pour que Description soit utilisé il faut l'ajouter à $UserInfo
        # Q.2.11 PasswordNeverExpires devient $true
        $UserInfo = @{
            Name                 = "$Prenom.$Nom"
            FullName             = "$Prenom.$Nom"
            Password             = $Password
            AccountNeverExpires  = $true
            PasswordNeverExpires = $true
	# Ajout de Description
	   Description           =$Description		           
}

        New-LocalUser @UserInfo
        #Q.2.10 remplacer utilisateur par user
        Add-LocalGroupMember -Group "User" -Member "$Prenom.$Nom"
        # Q.2.6 Affichage du mot de passe
         Write-Host "Le compte $Name a été créé avec le mot de passe $Pass" -ForegroundColor Green

        Write-Host "L'utilisateur $Prenom.$Nom a été crée"
    }
    # Q.2.9  Affiche un message en rouge si l'utilisateur existe déjà
        Write-Host "Le compte $Name existe déjà" -ForegroundColor Red
}
