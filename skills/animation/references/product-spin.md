# 3D Product Spin · `/animation product-spin`

> Famille : Styles 3D · #37

## C'est quoi
Un objet 3D qui tourne lentement sur lui-même sur fond propre : le packshot produit animé.

## Quand l'utiliser
- Présenter un produit (chaussure, flacon, téléphone), un objet de marque, un trophée.

## Technique clé
- **Vraie 3D** : `@remotion/three` (`ThreeCanvas`) + modèle `.glb` chargé via `useGLTF` (drei) depuis `staticFile`.
- Rotation : `rotation.y = frame * 0.02` ; ajouter un léger flottement `position.y = 0.05 * sin(frame*0.05)`.
- Lumière studio : `ambientLight 0.4` + `directionalLight` key (intensité 2) + rim light colorée derrière.
- Ombre de contact au sol (`ContactShadows`) ou ellipse floue.
- **Faux 3D sans modèle** : séquence d'images 360° (36 vues) affichée selon `Math.floor(frame/2) % 36`.

## Snippet Remotion (three.js)
```tsx
import {ThreeCanvas} from '@remotion/three';
import {AbsoluteFill, useCurrentFrame, useVideoConfig} from 'remotion';

export const ProductSpin: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const {width, height} = useVideoConfig();
  return (
    <AbsoluteFill style={{background: 'radial-gradient(circle at 50% 45%, #2b2f3a, #0b0c10 70%)'}}>
      <ThreeCanvas width={width} height={height} camera={{position: [0, 0.4, 4], fov: 40}}>
        <ambientLight intensity={0.4} />
        <directionalLight position={[3, 4, 5]} intensity={2} />
        <pointLight position={[-3, 1, -3]} intensity={30} color="#00B5C7" />
        <mesh rotation={[0.3, frame * 0.02, 0]} position={[0, 0.05 * Math.sin(frame * 0.05), 0]}>
          <torusKnotGeometry args={[0.8, 0.28, 220, 32]} />
          <meshStandardMaterial color="#EC4899" metalness={0.6} roughness={0.25} />
        </mesh>
      </ThreeCanvas>
    </AbsoluteFill>
  );
};
```
Dépendances : `npm i @remotion/three three @react-three/fiber` (et `@react-three/drei` pour `useGLTF`).

## Variations
- Remplacer le torus par ton modèle : `const {scene} = useGLTF(staticFile('produit.glb')); <primitive object={scene} />`.
- Rotation 180° + zoom avant sur un détail, puis texte en surimpression.
- Plateau tournant visible (cylindre) + reflets au sol.

## Pièges
- La rotation doit dépendre uniquement de `frame` (pas de `useFrame` de R3F qui accumule).
- Rendu 3D : utiliser `--gl=angle` ou `--gl=swangle` si le rendu headless est noir.
