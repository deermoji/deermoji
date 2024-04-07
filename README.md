### deermoji - A framework to create iOS apps with animatable animals faces
## What Is It
deermoji is a framework for Xcode 12+ for creating iOS apps with face detecting and following using animals 3D models.
## Example - How To Use
'''swift
import ARKit
import SwiftUI

struct ContentView: View {
    var body: some View {
        ARViewContainer()
            .edgesIgnoringSafeArea(.all)
    }
}

struct ARViewContainer: UIViewRepresentable {
    func makeUIView(context: Context) -> ARSCNView {
        let sceneView = ARSCNView()
        
        
        sceneView.delegate = context.coordinator
        
        
        sceneView.showsStatistics = true
        
        
        let scene = SCNScene()
        
        
        sceneView.scene = scene
        
        
        if let deerScene = SCNScene(named: "cat.obj") {
            let deerNode = deerScene.rootNode.childNode(withName: "cat", recursively: true)
            if let deerNode = deerNode {
                scene.rootNode.addChildNode(deerNode)
            }
        }
        
        
        let configuration = ARFaceTrackingConfiguration()
        sceneView.session.run(configuration)
        
        return sceneView
    }
    
    func updateUIView(_ uiView: ARSCNView, context: Context) {
        // No updates needed
    }
    
    func makeCoordinator() -> Coordinator {
        return Coordinator()
    }
    
    class Coordinator: NSObject, ARSCNViewDelegate {
        func renderer(_ renderer: SCNSceneRenderer, didUpdate node: SCNNode, for anchor: ARAnchor) {
            // Update the position and orientation of the deer node based on the face anchor
            guard let faceAnchor = anchor as? ARFaceAnchor else { return }
            guard let deerNode = node.childNodes.first else { return }
            
            let faceTransform = faceAnchor.transform
            deerNode.simdTransform = faceTransform
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
'''
