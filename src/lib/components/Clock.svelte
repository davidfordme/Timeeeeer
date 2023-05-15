<script lang="ts">
    export let isCounting = false

    const prependZero = (time : number) => {
        let prependedTime = time.toString();
        if (prependedTime.length == 1) prependedTime = '0' + prependedTime;
        return prependedTime;
    }

    const prependDoubleZero = (time : number) => {
        let prependedTime = time.toString();
        if (prependedTime.length == 1) prependedTime = '00' + prependedTime;
        if (prependedTime.length == 2) prependedTime = '0' + prependedTime;
        return prependedTime;
    }

    let timer : NodeJS.Timer;

    let hours : number = 0
    let minutes : number = 0
    let seconds : number = 0
    let miliseconds : number = 0

    let hoursString : string = prependZero(hours)
    let minutesString : string = prependZero(minutes)
    let secondsString : string = prependZero(seconds)
    let milisecondsString : string = prependZero(miliseconds)

    const onKeyDown = (e: KeyboardEvent) => {
        if(e.code === 'Space') {
            console.log("space")
            e.preventDefault()
            toggleTimer()
        }

        if(e.code === 'Escape') {
            resetTimer()
        }
    }

    const toggleTimer = () => {
        isCounting = !isCounting
        if(isCounting) startTimer()
        else stopTimer()
    }

    const startTimer = () => {
        timer = setInterval(() => {
            console.log('toggleTimer')
            
            miliseconds = miliseconds + 1

            if(miliseconds == 100) {
                miliseconds = 0
                seconds = seconds + 1
            }

            if(seconds == 60) {
                seconds = 0
                minutes = minutes + 1
            }

            if(minutes == 60) {
                minutes = 0
                hours = hours + 1
            }
            
            updateTimeStrings()
        }, 10)
    }

    const stopTimer = () => {
        clearInterval(timer)
    }

    const resetTimer = () => {
        isCounting = false
        hours = 0
        minutes = 0
        seconds = 0
        miliseconds = 0
        clearInterval(timer)
        updateTimeStrings()
    }

    const updateTimeStrings = () => {
        hoursString = prependZero(hours)
        minutesString = prependZero(minutes)
        secondsString = prependZero(seconds)
        milisecondsString = prependZero(miliseconds)
    }

</script>

<div class="clock{ isCounting ? ' counting' : '' }">
    {#if hours > 0 }
        <input
            name="hours"
            type="text"
            pattern="[0-9]*"
            bind:value={ hoursString }/><span>:</span>
    {/if}
    <input
        name="minutes"
        type="text"
        pattern="[0-9]*"
        bind:value={ minutesString } /><span>:</span>
    <input
        name="seconds"
        type="text"
        pattern="[0-9]*"
        bind:value={ secondsString } />

    <span class="mili">.{ milisecondsString }</span>
</div>

<svelte:window on:keydown={onKeyDown} />

<style lang="scss">
    .clock {
        position: relative;
        text-align: center;
        opacity: 0.5;
        display: flex;
        justify-content: center;
        align-items: center;

        &.counting {
            opacity: 1;
        }

        > * {
            flex: 0 1 auto;
            margin: 0;
            padding: 0;
            font-size: 10rem;
            line-height: 10rem;
            letter-spacing: -2rem;
            color: #FFFFFF;
            text-align: center;
	        font-family: 'Major Mono Display', monospace;
        }

        input {
            width: 230px;
            border: none;
            background: transparent;
        }

        .mili {
            position: absolute;
            bottom: -1rem;
            right: 0;
            font-size: 1rem;
            line-height: 1rem;
            letter-spacing: 0;
        }
    }
</style>