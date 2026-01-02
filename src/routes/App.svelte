<script>
    import favicon from "$lib/assets/favicon.svg";
    let count = $state(0);
    const digits = 6;
    const max = Math.pow(10, digits) - 1;
</script>

<div class="container">
    <h1><img src={favicon} alt="icon" />Abacus</h1>

    <div class="abacus">
        {#each { length: digits } as _, l}
            {@const pow = Math.pow(10, l)}
            {@const d = Math.floor(count / pow) % 10}
            <div class="digit">
                <div class="head">
                    {Math.pow(10, l)}
                </div>
                <div class="top">
                    <button
                        class={{ pearl: true, active: d >= 5 }}
                        onclick={(evt) => {
                            count += pow * (5 - 10 * (d >= 5));
                        }}>{5}</button
                    >
                    <div class="empty"></div>
                </div>
                <div class="bottom">
                    {#each { length: 4 } as _, f}
                        <button
                            class={{ pearl: true, active: d % 5 > f }}
                            onclick={(evt) => {
                                count += pow * (f + (d % 5 <= f) - (d % 5));
                            }}>{f + 1}</button
                        >
                    {/each}
                    <div class="empty"></div>
                </div>
                <div class="foot">
                    <button
                        class={"stepper"}
                        disabled={count < pow}
                        onclick={() => {
                            count -= pow;
                        }}>-</button
                    >

                    <span>{d}</span>
                    <button
                        class={"stepper"}
                        disabled={count + pow > max}
                        onclick={() => {
                            count += pow;
                        }}>+</button
                    >
                </div>
            </div>
        {/each}
    </div>
</div>

<style>
    :root {
        font-family: monospace;
        background-color: #111;
        color: #fff;
    }
    h1 {
        text-align: center;
        display: flex;
        gap: 0.75ex;
        align-items: center;
        justify-content: center;
    }
    h1 > img {
        display: block;
        width: 1em;
        height: 1em;
    }
    .abacus {
        display: flex;
        justify-content: center;
        flex-direction: row-reverse;
        gap: 2px;
        font-size: 1.2em;
        font-weight: bold;
        font-family: monospace;
    }
    .stepper {
        border: none;
        background: #000;
        padding: 0.5ex 1ex;
        margin: 0;
        flex-grow: 1;
        color: #fff;
        cursor: pointer;
    }
    .empty {
        flex-grow: 2;
        flex-shrink: 0;
        order: 10;
        background: transarent;
        flex-basis: 2em;
        text-align: center;
        color: #fff4;
        font-style: italic;
    }

    .digit {
        display: flex;
        flex-grow: 0;
        flex-direction: column;
        border: 2px solid #222;
        order: 5;
        background: #222;
        gap: 0.5ex;
        flex-basis: 10em;
    }
    .pearl {
        background: deeppink;
        border-radius: 2em;
        flex-basis: 1.5em;
        border: 2px solid hotpink;
        order: 20;
        text-align: center;
        opacity: 0.5;
        color: #fff;
        cursor: pointer;
    }

    .head {
        text-align: center;
        font-weight: bold;
        padding: 0.5ex;
        display: flex;
        align-items: end;
        justify-content: space-around;
        font-size: 0.8em;
        gap: 2em;
    }
    .foot {
        text-align: center;
        font-weight: bold;
        padding: 0.5ex;
        display: flex;
        align-items: center;
        justify-content: space-around;
        font-size: 0.8em;
        gap: 2em;
    }

    .bottom {
        display: flex;
        flex-direction: column;
        gap: 1ex;
        padding: 1ex;
        background: #444;
    }
    .top {
        display: flex;
        flex-direction: column-reverse;
        gap: 1ex;
        padding: 1ex;
        background: #444;
    }

    .active {
        order: 5;
        opacity: 1;
    }

    @media (max-width: 50em) {
        .abacus {
            flex-direction: column;
            width: max-content;
            margin: auto;
            align-items: center;
        }

        .digit {
            flex-direction: row;
        }
        .bottom {
            flex-direction: row;
        }
        .top {
            flex-direction: row-reverse;
        }
        .foot {
            flex-direction: column-reverse;
            text-orientation: sideways;
        }

        .head {
            writing-mode: vertical-lr;
            text-align: center;
            align-items: center;
        }
    }
</style>
