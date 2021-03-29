# Swift

import Foundation
import UIKit

public class GameViewController : UIViewController {
    let allCharactors = ["🐵", "👨‍🦲", "👻",]
    
    var myChoices = ["🐵", "👨‍🦲", "👻", ]
    var pcChoices = [String]()
    //
    var  myChoiceLabels = [UILabel]()
    var pcChoiceLabels = [UILabel]()
    var gameScoreLabel = UILabel()  //????
    var score = 0
    
    public override func loadView() {
        //setup pcChoice
        for _ in 0..<myChoices.count{
            pcChoices.append(allCharactors.randomElement()!)
            //!
        }
        
        //setup base view
        let view = UIView()
        view.backgroundColor = #colorLiteral(red: 0.1764705926, green: 0.4980392158, blue: 0.7568627596, alpha: 1)
        self.view = view
        
        //setup gameView
        let gameView = UIView()
        gameView.clipsToBounds = true   //设置为圆角
        gameView.layer.cornerRadius = 8.0   //圆角度数为8.0
        gameView.backgroundColor = #colorLiteral(red: 0.1411764771, green: 0.3960784376, blue: 0.5647059083, alpha: 1)
        self.view.addSubview(gameView)  //self主视图
        
        //setup image view
        let imageView = UIImageView()
        imageView.image = UIImage(named: "swift")
        imageView.clipsToBounds = true
        imageView.layer.cornerRadius = 4.0
        self.view.addSubview(imageView)
        
        //setup shuffleButton
        let shuffleButton = UIButton()
        shuffleButton.backgroundColor = .clear
        shuffleButton.setTitle("🎲", for: .normal)
        shuffleButton.titleLabel?.font = UIFont.systemFont(ofSize: 60)
        self.view.addSubview(shuffleButton)
        
        shuffleButton.addTarget(self, action: #selector(shuffle), for: .touchUpInside)
        
        //setup gameScoreLabel
        gameScoreLabel.text = "Score: \(score)"
        gameScoreLabel.textAlignment = .center
        gameScoreLabel.font = UIFont.systemFont(ofSize: 30)
        gameScoreLabel.textColor = #colorLiteral(red: 1.0, green: 1.0, blue: 1.0, alpha: 1.0)
        gameView.addSubview(gameScoreLabel)
        
        for index in 0..<myChoices.count{
            let myChoiceLabel = UILabel()
            let pcChoiceLabel = UILabel()
            
            setupMyChoiceLabel(for: myChoiceLabel, with: index)
            setupPCChoiceLabel(for: pcChoiceLabel, with: index)
            myChoiceLabels.append(myChoiceLabel)
            gameView.addSubview(myChoiceLabel)
            gameView.addSubview(pcChoiceLabel)
            
            myChoiceLabel.translatesAutoresizingMaskIntoConstraints = false
            pcChoiceLabel.translatesAutoresizingMaskIntoConstraints = false
            
            NSLayoutConstraint.activate([
                myChoiceLabel.widthAnchor.constraint(equalToConstant: 80),
                myChoiceLabel.heightAnchor.constraint(equalToConstant: 80),
                myChoiceLabel.topAnchor.constraint(equalTo: gameView.topAnchor, constant: CGFloat(120*(index)+80)),
                myChoiceLabel.leftAnchor.constraint(equalTo: gameView.leftAnchor, constant: 40),
                
                pcChoiceLabel.widthAnchor.constraint(equalToConstant: 80),
                pcChoiceLabel.heightAnchor.constraint(equalToConstant: 80),
                pcChoiceLabel.topAnchor.constraint(equalTo: gameView.topAnchor, constant: CGFloat(120*(index)+80)),
                pcChoiceLabel.rightAnchor.constraint(equalTo: gameView.rightAnchor, constant: -40),
            ])
        }
        
        gameView.translatesAutoresizingMaskIntoConstraints = false  //自动生成约束关闭
        imageView.translatesAutoresizingMaskIntoConstraints = false
        shuffleButton.translatesAutoresizingMaskIntoConstraints = false
        gameScoreLabel.translatesAutoresizingMaskIntoConstraints = false
        
        NSLayoutConstraint.activate([
            gameView.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            gameView.centerYAnchor.constraint(equalTo: view.centerYAnchor, constant: -10),  //向上移动
            gameView.widthAnchor.constraint(equalToConstant: 300),
            gameView.heightAnchor.constraint(equalToConstant: 460),

            imageView.topAnchor.constraint(equalTo: self.view.topAnchor, constant: 20),
            imageView.centerXAnchor.constraint(equalTo: self.view.centerXAnchor),
            imageView.widthAnchor.constraint(equalToConstant: 40),
            imageView.heightAnchor.constraint(equalToConstant: 40),
            
            shuffleButton.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            shuffleButton.topAnchor.constraint(equalTo: gameView.bottomAnchor, constant: 5),
            shuffleButton.widthAnchor.constraint(equalToConstant: 100),
            shuffleButton.heightAnchor.constraint(equalToConstant: 100),
            
            gameScoreLabel.centerXAnchor.constraint(equalTo: gameView.centerXAnchor),
            gameScoreLabel.topAnchor.constraint(equalTo: gameView.topAnchor, constant: 10),
            gameScoreLabel.widthAnchor.constraint(equalToConstant: 120),
            gameScoreLabel.heightAnchor.constraint(equalToConstant: 60),
        ])
    }
    
    func setupMyChoiceLabel(for label: UILabel, with index: Int) -> Void {
        label.text = myChoices[index]
        label.textColor = .black
        label.textAlignment = .center
        
        let battleResult = battle(between: myChoices[index], and: pcChoices[index])
        if battleResult == 1 {
            score += 1
            label.backgroundColor = #colorLiteral(red: 0.5843137503, green: 0.8235294223, blue: 0.4196078479, alpha: 1)
        } else if battleResult == 0 {
            label.backgroundColor = #colorLiteral(red: 0.9686274529, green: 0.78039217, blue: 0.3450980484, alpha: 1)
        } else {
            score -= 1
            label.backgroundColor = #colorLiteral(red: 0.9411764741, green: 0.4980392158, blue: 0.3529411852, alpha: 1)
        }
        label.clipsToBounds = true
        label.layer.cornerRadius = 8.0
        label.font = UIFont.systemFont(ofSize: 40)
    }
    
    func setupPCChoiceLabel(for label: UILabel, with index: Int) -> Void {
        label.text = pcChoices[index]
        label.textColor = .black
        label.textAlignment = .center
        
        let battleResult = battle(between: pcChoices[index], and: myChoices[index])
        if battleResult == 1 {
            score += 1
            label.backgroundColor = #colorLiteral(red: 0.5843137503, green: 0.8235294223, blue: 0.4196078479, alpha: 1)
        } else if battleResult == 0 {
            label.backgroundColor = #colorLiteral(red: 0.9686274529, green: 0.78039217, blue: 0.3450980484, alpha: 1)
        } else {
            score -= 1
            label.backgroundColor = #colorLiteral(red: 0.9411764741, green: 0.4980392158, blue: 0.3529411852, alpha: 1)
        }
        label.clipsToBounds = true
        label.layer.cornerRadius = 8.0
        label.font = UIFont.systemFont(ofSize: 40)
    }
    
    func battle(between a: String, and b: String) -> Int {
        if a == "👨‍🦲" && b == "🐵" {
            return 1
        } else if a == "👨‍🦲" && b == "👻" {
            return -1
        } else if a == "👻" && b == "🐵" {
            return -1
        } else if a == "👻" && b == "👨‍🦲" {
            return 1
        } else if a == "🐵" && b == "👨‍🦲" {
            return -1
        } else if a == "🐵" && b == "👻" {
            return 1
        } else {
            return 0
        }
    }
    
    @objc func shuffle() {
        pcChoices.removeAll()
        score = 0
        for _ in 0..<myChoices.count {
            pcChoices.append(allCharactors.randomElement()!)
        }
        
        for index in 0..<myChoices.count {
            let pcChoiceLabel = pcChoiceLabels[index]
            let myChoiceLabel = myChoiceLabels[index]
            
            setupPCChoiceLabel(for: pcChoiceLabel, with: index)
            setupMyChoiceLabel(for: myChoiceLabel, with: index)
        }
    }
}
