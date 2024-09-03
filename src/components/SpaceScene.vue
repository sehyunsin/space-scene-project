<template>
  <div>
    <div class="scene">
      <div class="sun"></div>
      <div v-for="(orbit, index) in orbits" :key="index" :id="`orbit${index + 1}`" class="orbit">
        <div :id="`planet${index + 1}`" class="planet"></div>
      </div>
    </div>

    <div class="stars"></div>
    <div class="comet"></div>
    <div class="aurora"></div>
  </div>
</template>

<script>
export default {
  name: 'SpaceScene',
  data() {
    return {
      orbits: [
        { width: 300, planetSize: 20, planetColor: '#3498db', orbitTime: 10 },
        { width: 500, planetSize: 30, planetColor: '#e74c3c', orbitTime: 20 },
        { width: 700, planetSize: 40, planetColor: '#2ecc71', orbitTime: 30 }
      ]
    }
  },
  mounted() {
    this.createStars();
    window.addEventListener('scroll', this.handleScroll);
    setInterval(this.createMeteor, 3000);
  },
  methods: {
    createStars() {
      const starsContainer = document.querySelector('.stars');
      for (let i = 0; i < 200; i++) {
        const star = document.createElement('div');
        star.classList.add('star');
        star.style.left = `${Math.random() * 100}%`;
        star.style.top = `${Math.random() * 100}%`;
        star.style.animationDelay = `${Math.random() * 5}s`;
        starsContainer.appendChild(star);
      }
    },
    createMeteor() {
      const meteor = document.createElement('div');
      meteor.classList.add('meteor');
      meteor.style.left = `${Math.random() * 100}%`;
      meteor.style.animationDuration = `${Math.random() * 1 + 1}s`;
      document.body.appendChild(meteor);

      setTimeout(() => {
        meteor.remove();
      }, 2000);
    },
    handleScroll() {
      const scrollPercentage = (window.scrollY / (document.documentElement.scrollHeight - window.innerHeight)) * 100;
      
      document.body.style.background = `linear-gradient(180deg, #000000 ${scrollPercentage}%, #0a4470 100%)`;
      
      const sun = document.querySelector('.sun');
      sun.style.transform = `translateY(${scrollPercentage}vh) scale(${1 + scrollPercentage/100})`;
      
      const orbits = document.querySelectorAll('.orbit');
      orbits.forEach((orbit, index) => {
        orbit.style.transform = `rotate(${scrollPercentage * (index + 1)}deg)`;
      });
    }
  },
  beforeUnmount() {
    window.removeEventListener('scroll', this.handleScroll);
  }
}
</script>

<style>
* {margin: 0;padding: 0;box-sizing: border-box;}

body {min-height: 500vh;background: #000;overflow-x: hidden;}

.scene {position: fixed;top: 0;left: 0;width: 100%;height: 100vh;display: flex;justify-content: center;align-items: center;}

.sun {width: 200px;height: 200px;background: radial-gradient(circle, #ff8a00, #ff4500);border-radius: 50%;box-shadow: 0 0 100px #ff4500;animation: pulse 4s infinite alternate;}

@keyframes pulse {
    0% { transform: scale(1); }
    100% { transform: scale(1.1); }
}

.orbit {position: absolute;border: 2px solid rgba(255, 255, 255, 0.1);border-radius: 50%;}

.planet {position: absolute;border-radius: 50%;}

#orbit1 {width: 300px;height: 300px;}
#planet1 {width: 20px;height: 20px;background: #3498db; animation: orbit 10s linear infinite;}
#orbit2 {width: 500px;height: 500px;}
#planet2 {width: 30px;height: 30px;background: #e74c3c;animation: orbit 20s linear infinite;}
#orbit3 {width: 700px;height: 700px;}
#planet3 {width: 40px;height: 40px;background: #2ecc71;animation: orbit 30s linear infinite;}

@keyframes orbit {
    0% { transform: rotate(0deg) translateX(150px) rotate(0deg); }
    100% { transform: rotate(360deg) translateX(150px) rotate(-360deg); }
}

.stars {position: fixed;top: 0;left: 0;width: 100%;height: 100%;pointer-events: none;}

.star {position: absolute;width: 2px;height: 2px;background: #fff;border-radius: 50%;animation: twinkle 5s infinite;}

@keyframes twinkle {
    0%, 100% { opacity: 0; }
    50% { opacity: 1; }
}

.comet {position: fixed;top: -10px;left: -10px;width: 5px;height: 5px;background: #fff;border-radius: 50%;box-shadow: 0 0 20px 5px #fff;animation: comet 10s linear infinite;}

@keyframes comet {
    0% { transform: translate(-10px, -10px) rotate(45deg); }
    100% { transform: translate(120vw, 120vh) rotate(45deg); }
}

.aurora {position: fixed;bottom: 0;left: 0;width: 100%;height: 200px;background: linear-gradient(0deg, rgba(97, 255, 238, 0.3) 0%, rgba(97, 255, 238, 0) 100%);filter: blur(20px);animation: aurora 20s linear infinite;}

@keyframes aurora {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
}

.meteor {position: fixed;width: 2px;height: 90px;background: linear-gradient(to bottom, rgba(255,255,255,0) 0%, #ffffff 100%);animation: meteor 2s linear infinite;}

@keyframes meteor {
    0% { transform: translateY(-100vh) translateX(100vw) rotate(45deg); }
    100% { transform: translateY(100vh) translateX(-100vw) rotate(45deg); }
}

@media (max-width: 768px) {
    .sun {
        width: 100px;
        height: 100px;
    }

    #orbit1 {
        width: 150px;
        height: 150px;
    }

    #orbit2 {
        width: 250px;
        height: 250px;
    }

    #orbit3 {
        width: 350px;
        height: 350px;
    }

    #planet1, #planet2, #planet3 {
        width: 10px;
        height: 10px;
    }

    @keyframes orbit {
        0% { transform: rotate(0deg) translateX(75px) rotate(0deg); }
        100% { transform: rotate(360deg) translateX(75px) rotate(-360deg); }
    }

    .aurora {
        height: 100px;
    }
}
</style>