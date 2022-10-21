<script lang="ts">
	import { text } from "svelte/internal";


    let form = {
        url: ""
    }

    let successMessage: HTMLHeadingElement;
    let textInput: HTMLInputElement;

    async function shortAndShow() {
        console.log("Shortening the URL...");
       // console.log(form)

        const url = "https://firebasedynamiclinks.googleapis.com/v1/shortLinks?key=" + import.meta.env.VITE_KEY;

        const data = {    
            "longDynamicLink": `https://s.molai.dev/?link=${form.url}`,
            "suffix": {
                "option": "SHORT"
            }
        }

        fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        }).then(async(response) => {
            const res: any = await response.json();
            navigator.clipboard.writeText(res.shortLink);
            successMessage.innerText = `URL Copied To Clipboard!`;
            setTimeout(() => {
                successMessage.innerText = "";
            }, 3500)
            textInput.value = "";
        });
    }
</script>

<!-- svelte-ignore a11y-missing-content -->
<div class="container">
<div class="content">
<h1>Molai URL Shortener</h1>
<form>
    <input autocomplete="off" placeholder="Enter URL..." type="text" name="url" id="url" bind:this={textInput} bind:value={form.url}>
    <input type="submit" on:click={(e) => {
        e.preventDefault();
        shortAndShow();
    }} value="Shorten URL">
    <p bind:this={successMessage}></p>
</form>
</div>
</div>

<style lang="scss">
    $box-shadow: 0 2px 10px 0 rgba(0, 0, 0, 0.15);

    .container {
        width: 100vw;
        height: 100vh;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 15px;
        background: lightcoral;

        .content {
            background: lightcyan;
            border-radius: 25px;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            gap: 15px;
            box-shadow: $box-shadow;
            height: 425px;
            width: 500px;

            p {
                font-size: 22px;
                transition: 350ms ease-in-out;
            }

            form {
                width: 350px;
                margin: 15px;
                height: 250px;
                display: flex;
                align-items: center;
                justify-content: center;
                flex-direction: column;
                gap: 15px;

                input[type=text] {
                    width: 350px;
                    height: 50px;
                    font-size: 18px;
                    border-radius: 25px;
                    padding: 15px;
                    padding-left: 22.5px;
                    outline: none;
                    border: none;
                    box-shadow: $box-shadow;
                }

                input[type=submit] {
                    width: 300px;
                    height: 50px;
                    font-size: 18px;
                    border-radius: 25px;
                    background: red;
                    color:white;
                    transition: 350ms ease-in-out;
                    cursor: pointer;
                    padding: 15px;
                    outline: none;
                    border: none;
                    box-shadow: $box-shadow;

                    &:hover {
                        background: darken(red, 11%)
                    }
                }
            }
        }
    }
</style>

