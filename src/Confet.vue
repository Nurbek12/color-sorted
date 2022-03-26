<template>
    <div class="stars">
        <span style="--sc: 1.4; --t: 13; --l: 3; --and: 1.3; --cl: purple;"></span>
        <span style="--sc: 1; --t: 30; --l: 10; --and: .9; --cl: purple;"></span>
        <span style="--sc: 1.2; --t: 15; --l: 20; --and: 1.6; --cl: purple;"></span>
        <span style="--sc: 1.1; --t: 65; --l: 10; --and: 1.3; --cl: yellow;"></span>
        <span style="--sc: 1.3; --t: 55; --l: 25; --and: 1.7; --cl: yellow;"></span>
        <span style="--sc: .9; --t: 35; --l: 5; --and: 1.9; --cl: yellow;"></span>
    </div>
    <div class="lines">
        <span style="--t: -15; --l: 13; --and: 2.3; --cl: purple;"></span>
        <span style="--t: 33; --l: 23; --and: 2.1; --cl: purple;"></span>
        <span style="--t: 23; --l: 10; --and: 1.2; --cl: purple;"></span>
        <span style="--t: 53; --l: 13; --and: 1.4; --cl: purple;"></span>
        <span style="--t: 13; --l: 3; --and: 1.9; --cl: purple;"></span>
    </div>
    <div class="confet">
        <span style="--bg: white; --t: 40; --l: 6; --and: .1; --x: 1;"></span>
        <span style="--bg: purple; --t: 47; --l: 10; --and: .12; --x: 1;"></span>
        <span style="--bg: red; --t: 58; --l: 23; --and: .15; --x: 1;"></span>
        <span style="--bg: blue; --t: 59; --l: 12; --and: .14; --x: -1;"></span>
        <span style="--bg: yellow; --t: 60; --l: 26; --and: .16; --x: -1;"></span>
        <span style="--bg: lime; --t: 0; --l: 23; --and: .16; --x: 1;"></span>
        <span style="--bg: brown; --t: 0; --l: 10; --and: .18; --x: 1;"></span>
        <span style="--bg: orange; --t: 0; --l: 3; --and: .13; --x: -1;"></span>
        <span style="--bg: gray; --t: 0; --l: 17; --and: .1; --x: -1;"></span>
    </div>
</template>

<script>
export default {

}
</script>

<style>
.stars, .lines, .confet{
    position: absolute;
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    z-index: 4;
    pointer-events: none;
}
.stars span{
    position: absolute;
    width: 10px;
    height: 10px;
    top: calc(1px*var(--t));
    left: calc(1px*var(--l));
    content: '';
    clip-path: polygon(0% 50%, 30% 30%, 50% 0%, 70% 30%, 100% 50%, 70% 70%, 50% 100%, 30% 70%);
    -webkit-clip-path: polygon(0% 50%, 30% 30%, 50% 0%, 70% 30%, 100% 50%, 70% 70%, 50% 100%, 30% 70%);
    /* filter: drop-shadow(0 0 5px #f00); */
    animation: star calc(1s*var(--and)) linear infinite;
    background: radial-gradient(#fff 10%, var(--cl) 55%, var(--cl) 100%);
}
.lines span{
    z-index: 4;
    position: absolute;
    height: 80px;
    width: 3px;
    top: calc(1px*var(--t));
    left: calc(1px*var(--l));
    clip-path: polygon(0% 50%, 50% 0%, 100% 50%, 50% 100%);
    -webkit-clip-path: polygon(0% 50%, 50% 0%, 100% 50%, 50% 100%);
    animation: lines calc(1s*var(--and)) linear infinite;
    background: radial-gradient(#fff 10%, var(--cl) 55%, var(--cl) 100%);
}
.confet span{
    position: absolute;
    width: 8px;
    height: 10px;
    top: calc(1px*var(--t));
    left: calc(1px*var(--l));
    opacity: 0;
    background: var(--bg);
    animation: confet calc(3s*var(--and)) linear forwards;
    animation-delay: calc(1s*var(--and));
}
@keyframes confet{
    0%{
        opacity: 0;
        transform: translate(0,40px);
    }
    30%{
        opacity: 1;
    }
    60%{
        transform: translate(0,-70px);
    }
    80%{
        transform: translate(calc(20px*var(--x)),-90px);
    }
    99%{
        transform: translate(calc(40px*var(--x)),-70px);
        opacity: 1;
    }
    100%{
        opacity: 0;
    }
}
@keyframes lines {
    0%,40%,60%,100%{
        opacity: 0;
    }
    50%{
        opacity: 1;
    }
}
@keyframes star {
    0%{
        transform: translateY(-10px) scale(var(--sc));
        opacity: 0;
    }
    50%,60%{
        opacity: 1;
    }
    100%{
        transform: translateY(30px) scale(var(--sc));
        opacity: 0;
    }
}
</style>