# Three.js com React: Guia Completo

## Introdução

Three.js é uma biblioteca JavaScript que permite criar e exibir gráficos 3D animados em um navegador web usando WebGL. Quando combinada com React, você pode criar experiências 3D interativas e imersivas em aplicações web modernas.

Este guia abordará desde conceitos básicos até técnicas avançadas para criar projetos profissionais com Three.js e React, além de sugestões de projetos que podem gerar renda extra.

## Índice

1. [Configuração do Ambiente](#configuração-do-ambiente)
2. [Conceitos Básicos do Three.js](#conceitos-básicos-do-threejs)
3. [Integração com React](#integração-com-react)
4. [Componentes Essenciais](#componentes-essenciais)
5. [Técnicas Avançadas](#técnicas-avançadas)
6. [Projetos Profissionais](#projetos-profissionais)
7. [Monetização](#monetização)
8. [Recursos Adicionais](#recursos-adicionais)

## Configuração do Ambiente

Para começar a trabalhar com Three.js e React, você precisa configurar seu ambiente de desenvolvimento:

### 1. Crie um projeto React

```bash
npx create-react-app meu-projeto-threejs
cd meu-projeto-threejs
```

### 2. Instale o Three.js e React Three Fiber

React Three Fiber é uma biblioteca que facilita a integração do Three.js com React:

```bash
npm install three @react-three/fiber @react-three/drei
```

O `@react-three/drei` é uma coleção de helpers úteis para React Three Fiber.

## Conceitos Básicos do Three.js

Antes de mergulhar no React, é importante entender os conceitos fundamentais do Three.js:

### Elementos Básicos

1. **Scene**: O contêiner para todos os objetos e luzes
2. **Camera**: Define a perspectiva do observador
3. **Renderer**: Renderiza a cena
4. **Mesh**: Combinação de geometria e material
5. **Geometry**: Define a forma do objeto
6. **Material**: Define a aparência do objeto
7. **Light**: Define a iluminação da cena

### Exemplo Básico com Three.js Puro

```javascript
import * as THREE from 'three';

function iniciarCena() {
  // Criar cena
  const scene = new THREE.Scene();
  
  // Criar câmera
  const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
  camera.position.z = 5;
  
  // Criar renderer
  const renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);
  document.body.appendChild(renderer.domElement);
  
  // Criar um cubo
  const geometry = new THREE.BoxGeometry();
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cube = new THREE.Mesh(geometry, material);
  scene.add(cube);
  
  // Função de animação
  function animate() {
    requestAnimationFrame(animate);
    
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    
    renderer.render(scene, camera);
  }
  
  animate();
}
```

## Integração com React

### React Three Fiber

React Three Fiber (R3F) é uma biblioteca que permite trabalhar com Three.js de forma declarativa em React. Ela lida com ciclos de renderização, atualizações de estado e recriação de objetos.

### Exemplo Básico com React Three Fiber

```jsx
import React, { useRef } from 'react';
import { Canvas, useFrame } from '@react-three/fiber';

function Box(props) {
  // Referência ao mesh
  const meshRef = useRef();
  
  // Hook para animação
  useFrame(() => {
    meshRef.current.rotation.x += 0.01;
    meshRef.current.rotation.y += 0.01;
  });
  
  return (
    <mesh
      {...props}
      ref={meshRef}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color={'orange'} />
    </mesh>
  );
}

export default function App() {
  return (
    <Canvas>
      <ambientLight intensity={0.5} />
      <pointLight position={[10, 10, 10]} />
      <Box position={[-1.2, 0, 0]} />
      <Box position={[1.2, 0, 0]} />
    </Canvas>
  );
}
```

### Estrutura Básica de um Componente R3F

```jsx
import React, { useRef, useState } from 'react';
import { Canvas, useFrame } from '@react-three/fiber';
import { OrbitControls } from '@react-three/drei';

function Cena() {
  return (
    <Canvas>
      {/* Controles de câmera */}
      <OrbitControls />
      
      {/* Iluminação */}
      <ambientLight intensity={0.5} />
      <directionalLight position={[10, 10, 5]} intensity={1} />
      
      {/* Objetos */}
      <mesh>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="royalblue" />
      </mesh>
    </Canvas>
  );
}
```

## Componentes Essenciais

### Câmera

```jsx
import { PerspectiveCamera } from '@react-three/drei';

function Cena() {
  return (
    <Canvas>
      <PerspectiveCamera makeDefault position={[0, 1, 5]} />
      {/* Resto da cena */}
    </Canvas>
  );
}
```

### Luzes

```jsx
function Iluminacao() {
  return (
    <>
      <ambientLight intensity={0.2} />
      <pointLight position={[10, 10, 10]} intensity={1} />
      <spotLight 
        position={[0, 10, 0]} 
        angle={0.3} 
        penumbra={0.8} 
        intensity={1.5} 
        castShadow 
      />
    </>
  );
}
```

### Materiais

```jsx
function ObjetosComMateriais() {
  return (
    <>
      {/* Material básico */}
      <mesh position={[-2, 0, 0]}>
        <sphereGeometry args={[1, 32, 32]} />
        <meshBasicMaterial color="red" wireframe />
      </mesh>
      
      {/* Material standard - responde à luz */}
      <mesh position={[0, 0, 0]}>
        <sphereGeometry args={[1, 32, 32]} />
        <meshStandardMaterial 
          color="blue" 
          roughness={0.4} 
          metalness={0.7} 
        />
      </mesh>
      
      {/* Material físico - ainda mais realista */}
      <mesh position={[2, 0, 0]}>
        <sphereGeometry args={[1, 32, 32]} />
        <meshPhysicalMaterial 
          color="green" 
          roughness={0.2} 
          metalness={0.9}
          clearcoat={1}
          clearcoatRoughness={0.1}
        />
      </mesh>
    </>
  );
}
```

### Geometrias

```jsx
function Geometrias() {
  return (
    <>
      {/* Cubo */}
      <mesh position={[-3, 0, 0]}>
        <boxGeometry args={[1, 1, 1]} />
        <meshStandardMaterial color="orange" />
      </mesh>
      
      {/* Esfera */}
      <mesh position={[-1, 0, 0]}>
        <sphereGeometry args={[0.7, 32, 32]} />
        <meshStandardMaterial color="lightblue" />
      </mesh>
      
      {/* Cilindro */}
      <mesh position={[1, 0, 0]}>
        <cylinderGeometry args={[0.5, 0.5, 1.5, 32]} />
        <meshStandardMaterial color="purple" />
      </mesh>
      
      {/* Torus (rosquinha) */}
      <mesh position={[3, 0, 0]}>
        <torusGeometry args={[0.5, 0.2, 16, 32]} />
        <meshStandardMaterial color="hotpink" />
      </mesh>
    </>
  );
}
```

## Técnicas Avançadas

### Carregando Modelos 3D

```jsx
import { useLoader } from '@react-three/fiber';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';

function Model() {
  const gltf = useLoader(GLTFLoader, '/modelos/meu-modelo.glb');
  
  return (
    <primitive 
      object={gltf.scene} 
      scale={[1, 1, 1]} 
      position={[0, 0, 0]} 
      rotation={[0, 0, 0]} 
    />
  );
}

// Uso no componente pai
function Cena() {
  return (
    <Canvas>
      <Suspense fallback={<LoadingBox />}>
        <Model />
      </Suspense>
    </Canvas>
  );
}

// Componente de carregamento
function LoadingBox() {
  return (
    <mesh>
      <boxGeometry args={[1, 1, 1]} />
      <meshBasicMaterial color="gray" wireframe />
    </mesh>
  );
}
```

### Interatividade

```jsx
import { useState } from 'react';

function ModeloInterativo() {
  const [hover, setHover] = useState(false);
  const [active, setActive] = useState(false);
  
  return (
    <mesh
      scale={active ? 1.5 : 1}
      onClick={() => setActive(!active)}
      onPointerOver={() => setHover(true)}
      onPointerOut={() => setHover(false)}>
      <boxGeometry args={[1, 1, 1]} />
      <meshStandardMaterial color={hover ? 'hotpink' : 'orange'} />
    </mesh>
  );
}
```

### Física com React Three Rapier

Primeiro, instale a biblioteca:

```bash
npm install @react-three/rapier
```

Agora, crie uma cena com física:

```jsx
import { Physics, RigidBody } from '@react-three/rapier';

function CenaComFisica() {
  return (
    <Canvas>
      <ambientLight intensity={0.5} />
      <Physics>
        {/* Chão */}
        <RigidBody type="fixed">
          <mesh position={[0, -1, 0]} rotation={[-Math.PI / 2, 0, 0]}>
            <planeGeometry args={[10, 10]} />
            <meshStandardMaterial color="gray" />
          </mesh>
        </RigidBody>
        
        {/* Cubo com física */}
        <RigidBody position={[0, 5, 0]}>
          <mesh>
            <boxGeometry />
            <meshStandardMaterial color="red" />
          </mesh>
        </RigidBody>
        
        {/* Esfera com física */}
        <RigidBody position={[1, 8, 0]}>
          <mesh>
            <sphereGeometry args={[0.5, 32, 32]} />
            <meshStandardMaterial color="blue" />
          </mesh>
        </RigidBody>
      </Physics>
    </Canvas>
  );
}
```

### Animações com React Spring

```bash
npm install @react-spring/three
```

```jsx
import { useSpring, animated } from '@react-spring/three';

function CuboAnimado() {
  const [active, setActive] = useState(false);
  
  const { scale, rotation, color } = useSpring({
    scale: active ? [1.5, 1.5, 1.5] : [1, 1, 1],
    rotation: active ? [0, Math.PI, 0] : [0, 0, 0],
    color: active ? 'royalblue' : 'hotpink',
    config: { mass: 1, tension: 170, friction: 26 }
  });
  
  return (
    <animated.mesh
      onClick={() => setActive(!active)}
      scale={scale}
      rotation={rotation}>
      <boxGeometry />
      <animated.meshStandardMaterial color={color} />
    </animated.mesh>
  );
}
```

### Pós-processamento

```jsx
import { EffectComposer, Bloom, ChromaticAberration } from '@react-three/postprocessing';

function CenaComEfeitos() {
  return (
    <Canvas>
      {/* Objetos da cena */}
      <mesh>
        <boxGeometry />
        <meshStandardMaterial color="white" emissive="white" emissiveIntensity={0.5} />
      </mesh>
      
      {/* Efeitos de pós-processamento */}
      <EffectComposer>
        <Bloom luminanceThreshold={0.5} luminanceSmoothing={0.9} height={300} />
        <ChromaticAberration offset={[0.005, 0.005]} />
      </EffectComposer>
    </Canvas>
  );
}
```

## Projetos Profissionais

### 1. Visualizador de Produtos 3D

Um visualizador interativo que permite aos clientes examinar produtos em 3D.

```jsx
import { useLoader } from '@react-three/fiber';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import { OrbitControls, PerspectiveCamera, Html } from '@react-three/drei';
import { Suspense, useState } from 'react';

function ProdutoVisualizador() {
  const [cor, setCor] = useState('black');
  const produtoModel = useLoader(GLTFLoader, '/modelos/produto.glb');
  
  // Função para mudar a cor do produto
  const mudarCor = (novaCor) => {
    produtoModel.scene.traverse((node) => {
      if (node.isMesh && node.material.name === 'CorPrincipal') {
        node.material.color.set(novaCor);
      }
    });
    setCor(novaCor);
  };
  
  return (
    <>
      <PerspectiveCamera makeDefault position={[0, 1, 5]} />
      <OrbitControls enablePan={false} maxPolarAngle={Math.PI / 2} />
      <ambientLight intensity={0.5} />
      <spotLight position={[10, 10, 10]} angle={0.15} penumbra={1} />
      
      <Suspense fallback={<Html>Carregando...</Html>}>
        <primitive object={produtoModel.scene} scale={1} />
      </Suspense>
      
      {/* UI para seleção de cores */}
      <Html position={[0, -1, 0]}>
        <div style={{ display: 'flex', gap: '10px' }}>
          <button onClick={() => mudarCor('black')} style={{ background: 'black', width: 20, height: 20 }} />
          <button onClick={() => mudarCor('red')} style={{ background: 'red', width: 20, height: 20 }} />
          <button onClick={() => mudarCor('blue')} style={{ background: 'blue', width: 20, height: 20 }} />
        </div>
      </Html>
    </>
  );
}

export default function App() {
  return (
    <div style={{ width: '100vw', height: '100vh' }}>
      <Canvas>
        <ProdutoVisualizador />
      </Canvas>
    </div>
  );
}
```

### 2. Configurador de Interiores 3D

```jsx
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Environment, useGLTF } from '@react-three/drei';
import { Suspense, useState } from 'react';

function Sala({ movelSelecionado, texturaParede }) {
  const { scene: salaModel } = useGLTF('/modelos/sala.glb');
  
  // Aplicar textura na parede
  salaModel.traverse((node) => {
    if (node.isMesh && node.name === 'Parede') {
      node.material.map = texturaParede;
      node.material.needsUpdate = true;
    }
  });
  
  return <primitive object={salaModel} />;
}

function Movel({ tipo, posicao }) {
  const { scene } = useGLTF(`/modelos/${tipo}.glb`);
  return <primitive object={scene} position={posicao} />;
}

function ConfiguradorInteriores() {
  const [moveis, setMoveis] = useState([]);
  const [texturaParede, setTexturaParede] = useState(null);
  
  const adicionarMovel = (tipo) => {
    setMoveis([...moveis, { id: Date.now(), tipo, posicao: [0, 0, 0] }]);
  };
  
  return (
    <div style={{ display: 'flex', height: '100vh' }}>
      <div style={{ width: '20%', padding: '20px', background: '#f0f0f0' }}>
        <h2>Móveis</h2>
        <button onClick={() => adicionarMovel('sofa')}>Adicionar Sofá</button>
        <button onClick={() => adicionarMovel('mesa')}>Adicionar Mesa</button>
        <button onClick={() => adicionarMovel('cadeira')}>Adicionar Cadeira</button>
        
        <h2>Texturas</h2>
        <div className="texturas">
          {/* Botões para selecionar texturas */}
        </div>
      </div>
      
      <div style={{ width: '80%' }}>
        <Canvas>
          <ambientLight intensity={0.5} />
          <spotLight position={[10, 10, 10]} angle={0.15} penumbra={1} />
          <OrbitControls />
          <Environment preset="apartment" />
          
          <Suspense fallback={null}>
            <Sala texturaParede={texturaParede} />
            {moveis.map((movel) => (
              <Movel key={movel.id} tipo={movel.tipo} posicao={movel.posicao} />
            ))}
          </Suspense>
        </Canvas>
      </div>
    </div>
  );
}
```

### 3. Portfólio 3D Interativo

```jsx
import { Canvas } from '@react-three/fiber';
import { OrbitControls, Text, Html } from '@react-three/drei';
import { useState, useRef } from 'react';

function ProjetoItem({ position, nome, descricao, imagem, onClick }) {
  const meshRef = useRef();
  const [hover, setHover] = useState(false);
  
  return (
    <mesh
      ref={meshRef}
      position={position}
      onClick={onClick}
      onPointerOver={() => setHover(true)}
      onPointerOut={() => setHover(false)}
      scale={hover ? 1.1 : 1}>
      <boxGeometry args={[1.5, 1, 0.1]} />
      <meshStandardMaterial 
        color={hover ? "#ff9900" : "#ffffff"}
        map={imagem}
      />
      <Text
        position={[0, -0.7, 0.1]}
        fontSize={0.1}
        color="black"
        anchorX="center">
        {nome}
      </Text>
    </mesh>
  );
}

function Portfolio3D() {
  const [projetoAtivo, setProjetoAtivo] = useState(null);
  
  const projetos = [
    { id: 1, nome: "Projeto A", descricao: "Descrição do projeto A...", posicao: [-2, 0, 0] },
    { id: 2, nome: "Projeto B", descricao: "Descrição do projeto B...", posicao: [0, 0, 0] },
    { id: 3, nome: "Projeto C", descricao: "Descrição do projeto C...", posicao: [2, 0, 0] },
  ];
  
  return (
    <>
      <Canvas camera={{ position: [0, 0, 5] }}>
        <ambientLight intensity={0.5} />
        <pointLight position={[10, 10, 10]} />
        <OrbitControls enablePan={false} maxPolarAngle={Math.PI / 2} minPolarAngle={Math.PI / 4} />
        
        {projetos.map((projeto) => (
          <ProjetoItem
            key={projeto.id}
            position={projeto.posicao}
            nome={projeto.nome}
            descricao={projeto.descricao}
            onClick={() => setProjetoAtivo(projeto)}
          />
        ))}
        
        <Text
          position={[0, 2, 0]}
          fontSize={0.5}
          color="#ffffff"
          anchorX="center">
          Meu Portfolio 3D
        </Text>
      </Canvas>
      
      {projetoAtivo && (
        <div className="projeto-detalhes" style={{
          position: 'absolute',
          bottom: '20px',
          left: '20px',
          backgroundColor: 'rgba(0,0,0,0.7)',
          color: 'white',
          padding: '20px',
          borderRadius: '5px',
          maxWidth: '400px'
        }}>
          <h2>{projetoAtivo.nome}</h2>
          <p>{projetoAtivo.descricao}</p>
          <button onClick={() => setProjetoAtivo(null)}>Fechar</button>
        </div>
      )}
    </>
  );
}
```

### 4. Jogos Web 3D

```jsx
import { Canvas } from '@react-three/fiber';
import { Physics, RigidBody, CuboidCollider } from '@react-three/rapier';
import { KeyboardControls, useKeyboardControls } from '@react-three/drei';
import { useRef, useState } from 'react';
import { useFrame } from '@react-three/fiber';

// Definição dos controles
const controlsMap = [
  { name: 'forward', keys: ['ArrowUp', 'KeyW'] },
  { name: 'backward', keys: ['ArrowDown', 'KeyS'] },
  { name: 'left', keys: ['ArrowLeft', 'KeyA'] },
  { name: 'right', keys: ['ArrowRight', 'KeyD'] },
  { name: 'jump', keys: ['Space'] }
];

function Player() {
  const bodyRef = useRef();
  const [, get] = useKeyboardControls();
  
  useFrame((state, delta) => {
    const { forward, backward, left, right, jump } = get();
    
    const impulse = { x: 0, y: 0, z: 0 };
    const torque = { x: 0, y: 0, z: 0 };
    
    const impulseStrength = 0.6 * delta;
    const torqueStrength = 0.2 * delta;
    
    if (forward) {
      impulse.z -= impulseStrength;
      torque.x -= torqueStrength;
    }
    
    if (backward) {
      impulse.z += impulseStrength;
      torque.x += torqueStrength;
    }
    
    if (left) {
      impulse.x -= impulseStrength;
      torque.z += torqueStrength;
    }
    
    if (right) {
      impulse.x += impulseStrength;
      torque.z -= torqueStrength;
    }
    
    bodyRef.current.applyImpulse(impulse);
    bodyRef.current.applyTorqueImpulse(torque);
    
    if (jump) {
      // Verificar se está no chão antes de pular
      // Implementação básica - em um jogo real, seria necessário raycasting ou outra verificação
      const position = bodyRef.current.translation();
      if (position.y < 1.5) {
        bodyRef.current.applyImpulse({ x: 0, y: 0.5, z: 0 });
      }
    }
  });
  
  return (
    <RigidBody ref={bodyRef} colliders="ball" restitution={0.2} friction={1}>
      <mesh castShadow>
        <sphereGeometry args={[0.5, 32, 32]} />
        <meshStandardMaterial color="blue" />
      </mesh>
    </RigidBody>
  );
}

function Nivel() {
  return (
    <>
      {/* Chão */}
      <RigidBody type="fixed">
        <mesh receiveShadow position={[0, -0.5, 0]}>
          <boxGeometry args={[20, 1, 20]} />
          <meshStandardMaterial color="green" />
        </mesh>
      </RigidBody>
      
      {/* Obstáculos */}
      <RigidBody type="fixed" position={[2, 0, 0]}>
        <mesh castShadow>
          <boxGeometry args={[1, 1, 1]} />
          <meshStandardMaterial color="red" />
        </mesh>
      </RigidBody>
      
      <RigidBody type="fixed" position={[-2, 0, 2]}>
        <mesh castShadow>
          <boxGeometry args={[1, 1, 1]} />
          <meshStandardMaterial color="red" />
        </mesh>
      </RigidBody>
    </>
  );
}

export default function JogoSimples() {
  return (
    <KeyboardControls map={controlsMap}>
      <Canvas shadows camera={{ position: [0, 5, 10], fov: 50 }}>
        <color attach="background" args={['#87ceeb']} />
        
        <ambientLight intensity={0.5} />
        <directionalLight
          castShadow
          position={[10, 10, 5]}
          intensity={1.5}
          shadow-mapSize={[2048, 2048]}
        />
        
        <Physics debug>
          <Player />
          <Nivel />
        </Physics>
      </Canvas>
      
      <div style={{ position: 'absolute', bottom: 20, left: 20, color: 'white' }}>
        <h3>Controles:</h3>
        <p>Setas ou WASD para mover</p>
        <p>Espaço para pular</p>
      </div>
    </KeyboardControls>
  );
}
```

## Monetização

Aqui estão várias maneiras de monetizar seus conhecimentos e projetos com Three.js e React:

### 1. Desenvolvimento de Experiências para E-commerce

Crie visualizadores de produtos 3D para lojas online. Muitas empresas estão dispostas a pagar por soluções que permitam aos clientes visualizar produtos em 3D antes da compra.

**Preço Estimado:** R$5.000 - R$20.000 por projeto

### 2. Criação de Portfólios 3D Interativos

Desenvolva portfólios interativos para designers, artistas, arquitetos e outros profissionais criativos.

**Preço Estimado:** R$3.000 - R$10.000 por site

### 3. Jogos Web 3D para Marketing

Crie jogos web simples para campanhas de marketing, onde os usuários interagem com produtos ou marcas de forma lúdica.

**Preço Estimado:** R$8.000 - R$25.000 por jogo

### 4. Visualizações de Dados 3D

Desenvolva painéis de dados tridimensionais para empresas que precisam visualizar informações complexas.

**Preço Estimado:** R$6.000 - R$15.000 por dashboard

### 5. Treinamentos e Cursos

Crie e venda cursos sobre Three.js e React, ensinando outros desenvolvedores.

**Preço Estimado:** R$200 - R$1.000 por aluno

### 6. Componentes e Templates React Three.js

Desenvolva e venda componentes reutilizáveis ou templates completos em plataformas como ThemeForest ou CodeCanyon.

**Preço Estimado:** R$50 - R$500 por licença

### 7. Consultoria

Ofereça serviços de consultoria para empresas que desejam implementar gráficos 3D em seus sites.

**Preço Estimado:** R$150 - R$300 por hora

### 8. Aplicativos de Realidade Virtual/Aumentada

Combine Three.js com WebXR para criar experiências de realidade virtual ou aumentada para web.

**Preço Estimado:** R$10.000 - R$30.000 por aplicativo

## Recursos Adicionais

### Bibliotecas Úteis

- **@react-three/fiber** - Reconciliador React para Three.js
- **@react-three/drei** - Coleção de helpers para R3F
- **@react-three/rapier** - Física para R3F
- **@react-spring/three** - Animações para R3F 
- **@react-three/postprocessing** - Efeitos de pós-processamento
- **zustand** - Gerenciamento de estado para aplicações React/Three.js

### Recursos de Aprendizado

- Documentação oficial do Three.js
- Documentação do React Three Fiber
- YouTube: canais como "Three.js Journey", "Wael Yasmina", "Bruno Simon"
- GitHub: exemplos e modelos de códigos
