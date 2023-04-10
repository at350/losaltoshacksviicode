<script>
        import * as THREE from "three";
    import { onMount } from 'svelte';

    let container;
    let camera, scene, renderer, particles;
    
    onMount(() => {
        init();

        return () => {
            renderer.dispose();
        };
    });
    
    function init() {
        scene = new THREE.Scene();
        camera = new THREE.PerspectiveCamera(
            75,
            container.clientWidth / container.clientHeight,
            0.1,
            1000
        );
        camera.position.z = 5;

        renderer = new THREE.WebGLRenderer({ alpha: true });
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(container.clientWidth, container.clientHeight);
        container.appendChild(renderer.domElement);

        const textureLoader = new THREE.TextureLoader();

        textureLoader.load(
            "/images/particle.png",
            (texture) => {
                const geometry = new THREE.BufferGeometry();
                const vertices = [];

                for (let i = 0; i < 5000; i++) {
                    vertices.push(THREE.MathUtils.randFloatSpread(2000));
                    vertices.push(THREE.MathUtils.randFloatSpread(2000));
                    vertices.push(THREE.MathUtils.randFloatSpread(2000));
                }

                geometry.setAttribute(
                    "position",
                    new THREE.Float32BufferAttribute(vertices, 3)
                );

                const material = new THREE.PointsMaterial({
                    color: 0xffffff,
                    size: 10,
                    map: texture,
                    blending: THREE.AdditiveBlending,
                    transparent: true,
                    depthWrite: false,
                });

                particles = new THREE.Points(geometry, material);
                scene.add(particles);

                animate();
            },
            undefined,
            (error) => {
                console.error("Error loading texture:", error);
            }
        );

        const resizeObserver = new ResizeObserver(() => {
            camera.aspect = container.clientWidth / container.clientHeight;
            camera.updateProjectionMatrix();
            renderer.setSize(container.clientWidth, container.clientHeight);
        });

        resizeObserver.observe(container);

        // Add mousemove event listener to the container
        container.addEventListener("mousemove", onMouseMove);
    }
 
    function animate() {
        requestAnimationFrame(animate);

        particles.rotation.x += 0.001;
        particles.rotation.y += 0.001;

        renderer.render(scene, camera);
    }

    function onWindowResize() {
        camera.aspect = container.clientWidth / container.clientHeight;
        camera.updateProjectionMatrix();

        renderer.setSize(container.clientWidth, container.clientHeight);
    }
</script>

<div id="background" bind:this={container}></div>

<style>
    #background {
    width: 100%;
    height: 120vh;
    position: absolute;
    top: 0px;
    left: 0;
    z-index: 1;
    background: #010721;
    }

</style>