<script lang="ts">
    import { T as Threlte } from "@threlte/core";
	import { createTransition, Float } from "@threlte/extras";
    import * as THREE from "three";
    import gsap from 'gsap';
	import { elasticOut } from "svelte/easing";

    export let position: [number, number, number] = [0,0,0];
    export let geometry: THREE.BufferGeometry = new THREE.IcosahedronGeometry(3);
    export let rate = 0.5;

    // Sound effects
    const soundEffects = [
        new Audio('/sounds/hit1.ogg'),
        new Audio('/sounds/hit2.ogg'),
        new Audio('/sounds/hit3.ogg'),
    ]

    let visible = false;

    // Material parameters for the shapes
    const materialParams = [
        { color: 0xff7f50, roughness: 0 },  // Light Orange
        { color: 0xff4500, roughness: 0.1 },  // Dark Orange
        { color: 0x228b22, roughness: 0.4 },  // Dark Green
        { color: 0x32cd32, roughness: 0.1 },  // Light Green
        { color: 0x008b8b, roughness: 0.1 },  // Teal
        { color: 0x4682b4, roughness: 0.1 },  // Light Teal
        { color: 0x2e8b57, roughness: 0.1 },  // Dark Teal
        { color: 0x4682b4, roughness: 0, metalness: 0.5 },  // Blue Gray
        { color: 0x2f4f4f, roughness: 0.1, metalness: 0.5 }  // Dark Blue Gray
    ];

    // Function to get a random material from the material parameters
    function getRandomMaterial() {
        return new THREE.MeshStandardMaterial(
            gsap.utils.random(materialParams));
    }

    function handleClick(event: MouseEvent) {
        gsap.utils.random(soundEffects).play();
        
        if (!('object' in event && event.object instanceof THREE.Mesh)) {
            return;
        }

        gsap.to(event.object.rotation, {
            x: `+=${gsap.utils.random(0, 3)}`,
            y: `+=${gsap.utils.random(0, 3)}`,
            z: `+=${gsap.utils.random(0, 3)}`,
            duration: 1.3,
            ease: 'elastic.out(1, 0.3)',
            yoyo: true,
        });

        event.object.material = getRandomMaterial();
    }

    // Bounce transition
    const bounce = createTransition((ref) => {
        return {
            tick(t) {
                if (t>0) visible = true;
                ref.scale.set(t, t, t)
            },
            easing: elasticOut,
            duration: gsap.utils.random(800, 1200),
            delay: gsap.utils.random(0, 500),
        }
    })

</script>

<Threlte.Group position={position.map((p)=> p * 2)}>
    <Float 
    speed={5 * rate} 
    rotationSpeed={5 * rate} 
    rotationIntensity={6 * rate} 

    floatIntensity={5 * rate}>
        <Threlte.Mesh 
        {visible} 
        {geometry} 
        in={bounce} 
        material={getRandomMaterial()}
        interactive
        on:click={handleClick}
        ></Threlte.Mesh>
    </Float>
</Threlte.Group>