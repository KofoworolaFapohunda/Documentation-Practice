# Technical Documentation for Additional Features

## FAQ Feature: 
This feature was added to the Frm battle file. It was created in frm battle design It involves creating a feature named FAQ.
The frequently asked questions features helps gives answers to some of the questions that may bother the users. It is activated when the user clicks the FAQ button. Just like most FAQ features will take you to the website of the game, this feature takes you to another page where the possibble questions and answers are located through the link provided. After creating the botton, its charateristics were specified and then the link that represents the link to the game's website where the FAQ were answered was added. The feature currently works well with the base code and there is no error caused by it.

### FAQ codeblock

A codeblock for the FAQ Feature is as described below:

``` cs
 private void btnHelp_Click(object sender, EventArgs e)
 {
     //Help
     string documentationUrl = "https://docs.google.com/document/d/158qKBqjiTSbWiRfbgNZ-8zu_gsyhuzam8IXES70mpeU/edit"; // link to google docs FAQ
     System.Diagnostics.Process.Start(documentationUrl);
 }

```
Since a button was created for the FAQ feature, the code below represent the generated code for window form
``` cs
     private System.Windows.Forms.Button btnHelp;//Button help design
```
 

## Background Music Feature:
This feature was a seperate file named Bgd Track.cs . It is meant to play until there is a hit and also continue to play if the game continues. The sound track was first added to the resources. The method for the music was created separately as a .cs file. This was then linked to the other files when they call the method since they are all in the same folder.

### Background Music Codeblock
A codeblock for the background music:

``` cs

    public static class BgdTrack
    {
        public static SoundPlayer bgdTrackPlayer;
        private static bool isBackgroundTrackPlaying = false;

        static BgdTrack()
        {
            // Initialize the background music player
            bgdTrackPlayer = new SoundPlayer(Resources.AssassinsCreed2); // Name of choice track in the Resources is used
        }
        public static void PlayBackgroundTrack()
        {
            if (bgdTrackPlayer != null && !isBackgroundTrackPlaying)
            {
                bgdTrackPlayer.PlayLooping();
                isBackgroundTrackPlaying = true;
            }
        }
        public static void StopBackgroundTrack()
        {
            if (bgdTrackPlayer != null && isBackgroundTrackPlaying)
            {
                bgdTrackPlayer.Stop();
                isBackgroundTrackPlaying = false;
            }
        }
    }
```
The code section below calls the Bgd Track.cs from the FrmBattle.cs and stops the music

```cs

public void SetupForBossBattle() {
  picBossBattle.Location = Point.Empty;
  picBossBattle.Size = ClientSize;
  picBossBattle.Visible = true;
  BgdTrack.StopBackgroundTrack();//Calling the background music to stop so that the bus_battle sound intro can play

        SoundPlayer simpleSound = new SoundPlayer(Resources.final_battle);
  simpleSound.Play();

  tmrFinalBattle.Enabled = true;
}
```
The code section below calls the Bgd Track.cs from the FrmBattle.cs and re-starts the music

``` cs
private void tmrFinalBattle_Tick(object sender, EventArgs e) {
  picBossBattle.Visible = false;
  tmrFinalBattle.Enabled = false;
        BgdTrack.PlayBackgroundTrack();//Calling the background music to start again
    }
```

## Escape button Feature
Escape button Feature: This Feature is added to the FrmBattle.cs file. It is activated when the user click the Escape button in “Fight!” dialog box and turn back to the level. This feature allow users to flee from a battle and keep the current player’s and enemy’s status. The feature currently works well with the base code and there is no error caused by it.

### Escape button code block
The code block of the Escape button is as described below

``` cs
private void btnEscape_Click(object sender, EventArgs e){
        instance = null;
        Close();

}

```

The code block of the Escape button in FrmBattle.Designer.cs is as described below
``` cs
this.btnEscape = new System.Windows.Forms.Button();
this.btnEscape.Font = new System.Drawing.Font("Microsoft Sans Serif", 14.25F, System.Drawing.FontStyle.Regular, System.Drawing.GraphicsUnit.Point, ((byte)(0)));
this.btnEscape.Location = new System.Drawing.Point(180, 637);
this.btnEscape.Margin = new System.Windows.Forms.Padding(4);
this.btnEscape.Name = "btnEscape";
this.btnEscape.Size = new System.Drawing.Size(192, 60);
this.btnEscape.TabIndex = 2;
this.btnEscape.Text = "Escape";
this.btnEscape.UseVisualStyleBackColor = true;
this.btnEscape.Click += new System.EventHandler(this.btnEscape_Click);
```

## Attack audio feature:
Introduction: The attack audio feature elevates the user's interaction with the application by introducing an audio, in the form of a "booom" sound effect, When a user initiates an attack, an auditory "boom" sound effect enhances the user experience

when the user initiates an attack action during the game. 


### Code Walkthrough for Attack Feature:

A plain codeblock:

```cs
private void btnAttack_Click(object sender, EventArgs e) {
    // Starting of audio playback for attack
    SoundPlayer attack_audio = new SoundPlayer(Resources.boom);
    player.OnAttack(-4);

    if (enemy.Health > 0) {
        // Playback of the attack audio
        attack_audio.Play();
        enemy.OnAttack(-2);
    }
    UpdateHealthBars();
    if (player.Health <= 0 || enemy.Health <= 0) {
        instance = null;
        Close();
        // Termination of the attack audio when no longer necessary
        attack_audio.Stop();
    }
}
```


Code in the Resources.Designer.cs 

``` cs
internal static System.IO.UnmanagedMemoryStream boom {
    get {
        return ResourceManager.GetStream("boom", resourceCulture);
    }
}
```

## Enemy Vanishing Feature
Introduction: The enemy vanishing feature helps the user in understanding which enemy he has killed after the attack. It makes the enemies disappear. The feature is functioning smoothly within the existing code, and it does not introduce any errors or issues.
It Called the method "Enemy_vanishing" in the "Fight" method.
It Created a new method "Enemy_vanishing" added functionality that makes the enemy disappear in the battle feild.
Did this in the FrmLevel.cs
This feature makes defeated enemies disappear from the screen so players can see which enemies they've defeated more easily.

### Code Walkthrough for Enemy Vanishing Feature:


``` cs
private void Fight(Enemy enemy) {
   player.ResetMoveSpeed();
   player.MoveBack();
   frmBattle = FrmBattle.GetInstance(enemy);
   frmBattle.Show();

   if (enemy == bossKoolaid) {
     frmBattle.SetupForBossBattle();
   }
         Enemy_vanishing(enemy); //calling the method here
     }
     private void Enemy_vanishing(Enemy enemy)
       //This method handles the disappearance of enemy graphics from the user interface upon their defeat.
     {
         if (enemy == enemyPoisonPacket)
         {
             picEnemyPoisonPacket.Visible = false;
         }
         else if (enemy == bossKoolaid && enemy.Health == 0)
         {
             picBossKoolAid.Visible = false;
         }
         else if (enemy == enemyCheeto)
         {
             picEnemyCheeto.Visible = false;
         }
     }
     
     
```

## Weapon item feature: 

This feature modified the code in FrmLevel.cs, FrmLevel.Designer.cs and FrmBattle.cs files and created a new file Weapon.cs. There will be a knife image on the level, when player hit the image, the image disappears, which means the player has picked up the knife as a weapon. After that, the attack damage of player doubles in battles. The feature currently works well with the base code and there is no error caused by it.

### Weapon feature code block

The code block of the Weapon Feature in FrmLevel.cs is as described below

``` cs
public Weapon weapon;
public static bool haveAWeapon;
private void FrmLevel_Load(object sender, EventArgs e) {
  const int PADDING = 8;
  const int NUM_WALLS = 13;

  player = new Player(CreatePosition(picPlayer), CreateCollider(picPlayer, PADDING));
  bossKoolaid = new Enemy(CreatePosition(picBossKoolAid), CreateCollider(picBossKoolAid, PADDING));
  enemyPoisonPacket = new Enemy(CreatePosition(picEnemyPoisonPacket), CreateCollider(picEnemyPoisonPacket, PADDING));
  enemyCheeto = new Enemy(CreatePosition(picEnemyCheeto), CreateCollider(picEnemyCheeto, PADDING));

  //weapon = new Weapon(CreatePosition(knife), CreateCollider(knife, PADDING));
      
  bossKoolaid.Img = picBossKoolAid.BackgroundImage;
  enemyPoisonPacket.Img = picEnemyPoisonPacket.BackgroundImage;
  enemyCheeto.Img = picEnemyCheeto.BackgroundImage;

  bossKoolaid.Color = Color.Red;
  enemyPoisonPacket.Color = Color.Green;
  enemyCheeto.Color = Color.FromArgb(255, 245, 161);

  walls = new Character[NUM_WALLS];
  for (int w = 0; w < NUM_WALLS; w++) {
    PictureBox pic = Controls.Find("picWall" + w.ToString(), true)[0] as PictureBox;
    walls[w] = new Character(CreatePosition(pic), CreateCollider(pic, PADDING));
  }

  Game.player = player;
  timeBegin = DateTime.Now;
}
  if (HitAWeapon(player))
        {          
            knife.Visible = false;
            haveAWeapon = true;             
        }
}
private bool HitAWeapon(Character c)
    {
        bool hitAWeapon = false;
        if (c.Collider.Intersects(weapon.Collider))
        {
            hitAWeapon = true;

        }
        return hitAWeapon;
    }

```

The code block of the Weapon Feature  in FrmLevel.Designer.cs is as described below
Some `code` goes here.

``` cs
this.knife = new System.Windows.Forms.PictureBox();
((System.ComponentModel.ISupportInitialize)(this.knife)).BeginInit();
this.knife.BackColor = System.Drawing.Color.Transparent;
this.knife.BackgroundImage = global::Fall2020_CSC403_Project.Properties.Resources.knife;
this.knife.BackgroundImageLayout = System.Windows.Forms.ImageLayout.Stretch;
this.knife.Location = new System.Drawing.Point(797, 244);
this.knife.Margin = new System.Windows.Forms.Padding(4);
this.knife.Name = "knife";
this.knife.Size = new System.Drawing.Size(81, 84);
this.knife.TabIndex = 0;
this.knife.TabStop = false;
this.Controls.Add(this.knife);
((System.ComponentModel.ISupportInitialize)(this.knife)).EndInit();

```
The code block of the Weapon feature in FrmBattle.cs is as described below

``` cs
private void btnAttack_Click(object sender, EventArgs e) {
  bool checkweapon = FrmLevel.haveAWeapon;
  if (checkweapon)
      {
          player.playerOnAttack(-8);
      }
      else {  
          player.playerOnAttack(-4);
      }
       
          
     

if (enemy.Health > 0) {
  enemy.enemyOnAttack(-2);
}

UpdateHealthBars();
if (player.Health <= 0 || enemy.Health <= 0) {
  instance = null;
  Close();
}
}

```

## Game Over Feature: 
This feature is added to Form GameOver file. It was cerated in Form GameOver design. It Involves creating a Text and making it displayed. This doalog pop's - up when the player or the enemy dies. It is useful as we can exit from the game after the death of the player or the enemy. I used window forms and a text tool to make this festure. I made sone editing in the text and some changes for font, size, alingnment and the color for this feature. The feature works well with the base code as of now and they are no errors caused by it.


### Creating a GameOver dialog pop - up
``` cs

public static int KillEnemy = 0;

UpdateHealthBars();
    if (enemy.Health <= 0)
    {
        instance = null;
        KillEnemy++;
        Close();
        if(KillEnemy == 3)
        {
            gameover = Gameover.GetInstance();
            gameover.Show();
        }
    }
    if(player.Health <= 0)
    {
        instance = null;
        Close();
        gameover = Gameover.GetInstance();
        gameover.Show();
    }

```

## Restart Feature: 
This feature is added to Form GameOver file. It was created in Form GaveOver design. It involves creating a button and making it functional. It is very helpful as we can restart the game after completing once or after the player dies. I used window forms and a button tool to create this feature. I made some changes regarding the font, size and color for this feature to make it look presentable. The feature works well with the base code as of now and they are no errors caused by it.

### Creating a restart button
``` cs
private void Restart_Click(object sender, EventArgs e)
{
    Application.Restart();
}

```
## Select Character Feature: 
Description:
The Select Character feature allows users to choose their player character from a list of three options in the FrmSelectCharacter.cs form. Once selected, the chosen character appears in the FrmBattle.cs game, seamlessly integrating with the existing codebase without introducing any errors.

### Implementation Details of Select Character Feature:

Form Creation:

Created a new form named FrmSelectCharacter.cs to manage character selection.
Integrated picture boxes to visually represent each available character option.

### User Interaction of Select Character Feature :

Enabled user interaction by assigning control to the picture boxes.
Implemented click events for the picture boxes to detect user selection.
Dynamic Character Display:

Programmed the functionality to display the selected character in the FrmBattle.cs game.
Ensured smooth integration with existing codebase.

### Feature to Change the Theme of the Game 

Description:
The Change Theme feature introduces a new winter theme to the game, transforming the appearance of enemies, the main character, and the game tiles. This enhancement aims to provide a fresh and immersive gaming experience with a winter setting.

### Implementation Details of Changing Theme Feature:

Image Replacement:

Replaced existing images of enemies with winter-themed counterparts to maintain thematic consistency.
Updated player character images to reflect the winter theme, enhancing visual coherence.

Tile Modification:

Added new winter-themed tiles to the game, contributing to the overall aesthetic transformation.
Ensured seamless integration of the new tiles into the existing game environment.
Code Modification:

Updated relevant code sections to accommodate the changes in images and tiles.
Verified that the alterations did not introduce any errors or disrupt the existing game functionality.

## Exit button Feature:

This Feature is added to the FrmBattle.cs, FrmBattle.Designer,cs files. It is activated when the user clicks on the Exit button in GameOver Screen to exit the game.Also changed the background for the GameOver Screen to make it look presentable. This feature currently works well and there is no error caused by it.

### Implementation Details for Exit button:
Button Creation:

Created a new button in YouWin.cs form to manage the action.
Implemented click events for the button.
Programmed the functionality to be able to exit the game.
Ensured smooth integration with existing codebase.

Code:
Application.exit();

## Next Button Feature:

Description:
The next button feature allowers user to enter the next level after he successfully defeated the enemy.

### Implementation details of Next Button Feature:

Created a new button in Gameover.cs form to manage the action.
Implemented click funtion events for the button.
Programmed the functionality to be able to enter the next level of the game.
Ensured smooth integration with existing codebase.

## Level 2 Feature:

Description:
The Level 2 Feature allows the user to enter a different level of the game.

### Implementaion Details of Level 2 Feature:

Form Creation:

Created a new form named FrmLevel2.cs to start the level.
Integarted picture boxes to visulayy represent each available charcters of the level.
Designed a different theme along with different characters to the form.

Created another Form named FrmBattle2 to manage battles between the Characters.

Programmed the functionality to be enable movemnet of the player.
Ensured smooth integration with the existing codebase.
This feature currently works well and there is no error caused by it.

## Healing item Feature

Description:

This feature adds the item class “potion” to the game, player can consume 1 potion to gain 5 HP in battle by clinking the button “Heal”. Player have 1 potion at the beginning of the game, and can pick up potions in levels. Player can check how many potions they have in battles.

### Implementation Details:

Establish a new class named “HealingItem”, this class inherits the class “Character”. This new class allow the potion to appear in levels, and record how many potions player already have.

 Add a new button in the form “FrmBattle”, so player can use the potion to heal in battles.

## Experience System Feature.

Description:

This feature allow player to get experience in battles. Player can get 1 point of experience by an attack, and get 10 point by a kill. When the experience reaches max experience, player will be upgraded for more strength, max HP and max EXP.There’s a bar to show how much experience the player have and how much will it take to reach next level. There is a label to show the current level.

### Implementation Details:

Separate the class “BattleCharacters” into “Player” and “Enemy” to distinguish player and enemy, because player can get experience and level up but enemy cannot.

Add new elements to the class “Player”, the player now have experience system.
       
Add new controls in the form “FrmBattle”, so player can get experience in battles.

player.UpdateExp(1); // Player get 1 Exp when attack.

player.UpdateExp(10); //Player get 10 Exp when kill.

Add a bar and a label in the form “FrmBattle” to visualize the experience and current level.

## You Win Feature
Description:
Instead of the form “GameOver”, “You Win” shows up when player kills all enemies, and player can choose whether to leave the game or something.

### Implementation Details of You Win Feature:

Add a new Form “YouWin”

## Player Status Feature
Description:
Players now can directly check their current HP, EXP and level on the levels.

### Implementation Details:

In the form “FrmLevel”, add 2 bars to show the HP and EXP respectively, add a label to show the current level.

## Welcome Page
Description
The welcome page is the first page the player gets to. From the welcome page, the player can either start the game, exit the game or watch a tutorial video on how to navigate the game.

## Implementation of the Welcome Page Feature
Created a new form named welcome.cs
Add the picture to the background and all the three buttons named the exit, start and tutorial page
This was linked to the program.cs file so that it will appear first
The exit button uses the application.exit() to shut down the program
The start button was linked to the FrmSelectCharacter.cs
The tutorial video was linked to the Url

### Advert Feature
The advert feature is to advertise  different games that the players may decide to choose from when he or she finishes the Dr Peanut Game

## Implementation of the Advert Feature
Created a new form named advert.cs
Add the picture to the background and the exit button at the corner of the picture
Add the link to and video tutorial page
This was linked to the program.cs file so that it will appear first
The exit button uses the application.exit() to exit the advert of games incase the player wants to replay the game.

## Conclusion
So far, all the added features are good and there is no code break caused by the features.
