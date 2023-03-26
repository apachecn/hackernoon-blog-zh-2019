# 如何从零开始制作一款机器学习安卓游戏

> 原文：<https://medium.com/hackernoon/how-to-make-a-machine-learning-android-game-from-scratch-82d9406a7635>

让我们从零开始做一个**安卓机器学习游戏**🌟。

你对**机器学习**着迷吗？你喜欢 **Java 还是安卓**？要不要用安卓和机器学习做一个**酷的项目？**

![](img/21f08747bac331db0ace5f6e166c2732.png)

machine learning

如果是，那么你肯定是来对地方了。在这里，我们将制作一个使用机器学习来击败用户的 android 应用程序。

是的，失败！！这意味着当你玩下去的时候，游戏和你的技术会变得越来越专业，所以你最好做好失败的准备。

但即使输了，你也一定会骄傲。你会问为什么？

嗯，**你让它**学会了，你让它在游戏里专业了。那么，谁是老板？

出于开始的目的，让我们从一个简单的游戏开始，可能性较小。我认为像石头、布、剪刀这样的东西就可以了。

这篇帖子将帮助你了解如何用**机器学习和 android** 制作一个游戏，这样你就可以制作一个自己的令人惊叹的项目游戏，在这之后！

让我们通过一个简单的流程图来深入了解我们的应用程序将如何发展。

Android 应用程序→给出石头/布/剪刀的选项→用户选择其中一个→如果是第一次，则由计算机随机选择→否则，计算机分析用户输入，并使用统计选择获胜概率更大的一个→游戏继续，计算机变得更专业。

这是我们的应用程序流程图，当我们陷入困境时，它会给我们指引方向。

***简单开发者提示:*** *在制作一个 app 之前，一定要做一个简单的流程图，在制作的过程中指导你的旅程。*

> 现在让我们深入一些代码。

在 Android Studio 或你选择的 IDE 上开始一个新项目。头朝着**MainActivity.java 的文件**。我们将首先从 Java 代码开始。

让我们首先初始化一些变量和标志。因此，在您的 **MainActivity** 中添加以下代码。

```
/*first two are for previous result second two are for recent result
 *results[0][x][0][x] 0 at index 0 or 2 is a win
 *results[1][x][1][x] 1 at index 0 or 2 is a tie
 *results[2][x][2][x] 2 at index 0 or 2 is a loss
 *results[x][0][x][0] 0 at index 1 or 3 is for rock
 *results[x][1][x][1] 1 at index 1 or 3 is for scissors
 *results[x][2][x][2] 2 at index 1 or 3 is for paper*/

private int[][][][][][] results6D = new int[3][3][3][3][3][3];
private int[][][][] results4D = new int[3][3][3][3];
private int[][] results2D = new int [3][3];

private int lastResult =- 1;
private int lastCompMove =- 1;

private int secondResult =- 1;
private int secondCompMove =- 1;

private int status = 0;
```

为了更好的理解，我在代码中做了注释。我们将使用用户最近的选择来训练我们的**机器学习模型**。

现在我们应该添加代码，让用户选择**从石头、布、剪刀**中选择。还有显示计算机画了什么和游戏结果的代码。

像这样的代码将为我们游戏的上述方面做些什么，这也是我们代码的`userMove(view)`方法中的代码。

```
int userMoveInt;

Button b = (Button) v;
String name = b.getText().toString();

switch (name) {
    case ("Rock"): {
        userMoveInt = 0;
        break;
    }
    case ("Paper"): {
        userMoveInt = 1;
        break;
    }
    default:
        userMoveInt = 2;
}

int compMoveInt;
String compMoveStr;
int resultStatus;
compMoveInt = nextCompMove();

if (compMoveInt == 0)
    compMoveStr = "rock";

else if (compMoveInt == 1)
    compMoveStr = "paper";

else if (compMoveInt == 2)
    compMoveStr = "scissors";

else
    compMoveStr = "nothing";

String text = "I drew "+compMoveStr+", ";
resultStatus = checkResult(compMoveInt,userMoveInt);

if (resultStatus == 0)
    text+="You Win!";

else if (resultStatus == 1)
    text += "It's a Tie.";

else if (resultStatus == 2)
    text += "You Lose!";

TextView output = findViewById(R.id.*output*);
output.setText(text);
trainModel(resultStatus,compMoveInt);

float[] percentages = analyzeResults();
int[] changes = changes(percentages);
int rockChance = changes[0];
int paperChance = changes[1];

text = "Rock: "+String.*valueOf*(rockChance)+"%";((TextView) findViewById(R.id.*rPer*)).setText(text);
text = "Paper: "+String.*valueOf*(paperChance)+"%";((TextView) findViewById(R.id.*pPer*)).setText(text);
text = "Scissors: "+String.*valueOf*(100-rockChance-paperChance)+"%";((TextView) findViewById(R.id.*sPer*)).setText(text);

int top = Math.*max*(rockChance,Math.*max*(paperChance,100-rockChance-paperChance));
text = "Confidence\n"+String.*valueOf*(Math.*round*(2*(top-50)))+"%";((TextView) findViewById(R.id.*confidence*)).setText(text);
```

此外，我们使用整数 0 代表石头，1 代表布，2 代表剪刀。

现在让我们写一些代码让**计算机选择**和**向用户**学习。此外，这段代码位于我们代码的方法`nextCompMove()`中。

```
if (lastResult == -1) // that's default for the paper
    return 1;

int chanceOneHundred = (int)(Math.*random*()*99)+1;
float[] percentages = analyzeResults();
int[] changes = changes(percentages);
int rockChance = changes[0];
int paperChance = changes[1];
int compMove;

if (chanceOneHundred < rockChance)
    compMove = 0;

else if (chanceOneHundred<rockChance+paperChance)
    compMove = 1;

else
    compMove = 2;

return compMove;
```

如果你懂一点概率，那么你知道，随机选择石头、布、剪刀中的任何一个，都有概率 **1/3 或者~33%** 。

所以，我们把每个选择的默认几率设为 33%。**的代码改变了**的选择，因为方法`changes(percentage)`看起来像这样:

```
int totalPercentages = (int) (percentages[0]+percentages[1]+percentages[2]);
int rockChance = 33;
int paperChance = 33;

if (totalPercentages != 0) {
    rockChance= (int) (100*percentages[0]/totalPercentages);
    paperChance= (int) (100*percentages[1]/totalPercentages);
}

return new int[]{rockChance, paperChance};
```

现在让我们**帮助计算机分析用户**的移动，并为计算机选择最有可能赢得游戏的东西。

这将是我们对方法`analyzeResults()`的代码:

```
float[] percentages=new float[3];
        if (lastResult == -1)
            return percentages;

        int[][] relevantResults;
        //noinspection ConstantConditions

            if (lastResult!=-1) {

            int[][] resultsTo2D;
            int[][] results4DTo2D = results4D[lastResult][lastCompMove];
            if (secondResult!=-1) {
                int[][] results6DTo2D=results6D[secondResult][secondCompMove][lastResult][lastCompMove];
                resultsTo2D = *totalAmount*(results6DTo2D)<*totalAmount*(results4DTo2D)&&*totalAmount*(results6DTo2D)<9?results4DTo2D:results6DTo2D;
            }

            else
                resultsTo2D = results4DTo2D;

            relevantResults = *totalAmount*(resultsTo2D)<*totalAmount*(results2D)&&*totalAmount*(resultsTo2D)<1?results2D:resultsTo2D;
        }
        else
            relevantResults = results2D;
        for (int i=0;i<percentages.length;i++) {

//       percentages[i]=1+(relevantResults[2][i]*4+relevantResults[1][i]-relevantResults[0][1]*2);
            percentages[i]=1+(relevantResults[2][i]+relevantResults[0][i==2?0:i+1])*4-(relevantResults[0][i]+relevantResults[2][i==0?2:i-1])*4;
            if (percentages[i] < 0)
                percentages[i]=0;
        }
        return percentages;
```

花些时间理解这段代码，它几乎是**不言自明的**。如果你理解了这一点，你就几乎理解了游戏的全部代码。

让我们也完成`trainModel(resultStatus, compMove)`代码，这样我们就完成了游戏的**机器学习关键部分**。

```
if (lastResult != -1) {
    results4D[lastResult][lastCompMove][resultStatus][compMoveInt]++;

    if (secondResult != -1)
        results6D[secondResult][secondCompMove][lastResult][lastCompMove][resultStatus][compMoveInt]++;

    secondResult=lastResult;
    secondCompMove=lastCompMove;
}

results2D[resultStatus][compMoveInt]++;
lastResult=resultStatus;
lastCompMove=compMoveInt;
```

你就完事了。你给自己做了一个机器学习的安卓游戏，几乎**当然赢了**对用户。

但是，等等！**获胜的部分在哪里？我们当然不想让用户对谁赢了游戏一无所知，不是吗？**

所以，让我们为`checkResult(compMove, userMove)`写代码，告诉用户游戏的输赢。

```
/*Status 0 is a win
 *Status 1 is a tie
 *Status 2 is a loss*/
int resultStatus;
if (compMoveInt == 0 && userMoveInt == 1 || compMoveInt == 1 && userMoveInt == 2 || compMoveInt == 2 && userMoveInt == 0) {
    resultStatus = 0;
    TextView wins= findViewById(R.id.*wins*);
    int winAmount=Integer.*parseInt*(wins.getText().toString());
    wins.setText(String.*valueOf*(winAmount+1));
}
else if (compMoveInt == userMoveInt) {
    resultStatus = 1;
    TextView ties = findViewById(R.id.*ties*);
    int tieAmount = Integer.*parseInt*(ties.getText().toString());
    ties.setText(String.*valueOf*(tieAmount+1));
}
else {
    resultStatus = 2;
    TextView losses = findViewById(R.id.*losses*);
    int lossAmount = Integer.*parseInt*(losses.getText().toString());
    losses.setText(String.*valueOf*(lossAmount+1));
}

return resultStatus;
```

是的，现在你已经为你的机器学习机器人游戏编写了你需要的所有代码，并打败了任何人。

UI 和代码中的一些其他微调，可以在我的 **GitHub 库**中找到。

[**app**](https://github.com/Pradyuman7/MachineLearning-Game)的完整源代码。

毫无疑问，你现在已经完成了。

你还在等什么？去给你的朋友们展示一下 ***他们如何可怜地输给你的安卓游戏*** 。

**去炫耀吧！**

[**阅读我之前关于轻松制作一个简单的动画和干净的网站的帖子。**](https://hackernoon.com/how-to-make-a-cool-introduction-webpage-for-yourself-easily-177f0b7fe1e3)