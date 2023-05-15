<script lang="ts">
    export let isCounting = false

    let timer : NodeJS.Timer;

    let hours : number = 0
    let minutes : number = 0
    let seconds : number = 0

    let hoursString : string = ''
    if(hours > 1) hoursString =  ((hours < 10) ? '0' : '') + hours

    let minutesString = ((minutes < 10) ? '0' : '') + minutes
    let secondsString = ((seconds < 10) ? '0' : '') + seconds

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
            console.log("furtherTimer")
            seconds = seconds + 1
            if(seconds > 60) {
                seconds = 0
                minutes = minutes + 1
            }

            if(minutes > 60) {
                minutes = 0
                hours = hours + 1
            }
            
            updateTimeStrings()
        }, 1000)
    }

    const stopTimer = () => {
        clearInterval(timer)
    }

    const resetTimer = () => {
        isCounting = false
        hours = 0
        minutes = 0
        seconds = 0
        clearInterval(timer)
        updateTimeStrings()
    }

    const updateTimeStrings = () => {
        hoursString = prependZero(hours)
        minutesString = prependZero(minutes)
        secondsString = prependZero(seconds)
    }

    const prependZero = (time : number) => {
        let prependedTime = time.toString();
        if (prependedTime.length == 1) prependedTime = '0' + prependedTime;
        return prependedTime;
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
</div>

<svelte:window on:keydown={onKeyDown} />

<style lang="scss">
    .clock {
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
            color: #FFFFFF;
            text-align: center;
	        font-family: 'Major Mono Display', monospace;
        }

        input {
            width: 230px;
            border: none;
            background: transparent;
        }
    }
</style>