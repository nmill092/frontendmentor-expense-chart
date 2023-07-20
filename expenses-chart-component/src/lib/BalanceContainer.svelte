<script>
    let balance = 921.48; 
    import logo from '../assets/logo.svg';
</script>

<div class="container">
    <div class="balance-wrapper">
        <div class="left">
            <span>My Balance</span>
            <span class="balance">${balance}</span>
        </div>
        <div class="right">
            <img alt="Balance app logo" src={logo}/>
        </div>
    </div>
</div>


<style lang="scss">
   @import '/styles/_variables.scss';
    .container {
        border-radius: $br; 
        background-color: $soft-red;
        height: 100%; 
        width: 100%; 
        color: $pale-orange; 
    }

    .balance-wrapper {
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 100%; 
        padding: 1rem 2rem;

        span {
                display: block;
        }

        .left {
            display: flex;
            gap: .3rem;
            flex-direction: column;
        }
    }

    .balance {
        font-weight: $fw-bold;
        font-size: calc($root-fs * 1.8);
    }
</style>