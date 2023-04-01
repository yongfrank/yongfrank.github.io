---
title: "SpriteKit"
date: 2023-03-29T01:36:34+08:00
---

###

```swift
import SpriteKit
import GameplayKit

class GameScene: SKScene {
    override func didMove(to view: SKView) {
        let background = SKSpriteNode(imageNamed: "background")
        
        // https://developer.apple.com/design/human-interface-guidelines/foundations/layout#platform-considerations
        // Remember, unlike UIKit SpriteKit positions things based on their center
        // – i.e., the point 0,0 refers to the horizontal and vertical center of a node.
        // iPad mini screen size: 1024x768
        // var position: CGPoint { get set } .position The default value is (0.0,0.0).
        background.position = CGPoint(x: 512, y: 384)
        
        // The blend mode used to draw the sprite into the parent’s framebuffer.
        // The source color replaces the destination color.
        // The .replace option means "just draw it, ignoring any alpha values,"
        background.blendMode = .replace
        
        background.zPosition = -1
        
        // Adds a node to the end of the receiver’s list of child nodes.
        addChild(background)
        physicsBody = SKPhysicsBody(edgeLoopFrom: frame)
    }
    override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
        if let touch = touches.first {
            let location = touch.location(in: self)
            let box = SKSpriteNode(imageNamed: "ballBlue")
//            let box = SKSpriteNode(color: .red, size: CGSize(width: 64, height: 64))
            box.position = location
            
            // img @2x 88 x 88 ==> 44 x 44 ==> dimensions = 22
            box.physicsBody = SKPhysicsBody(circleOfRadius: 22)
//            box.physicsBody = SKPhysicsBody(rectangleOf: CGSize(width: 64, height: 64))
            addChild(box)
        }
    }
}

```
