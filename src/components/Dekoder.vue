<template>
  <div id="dekoder">
    <div class="container">
      <div class="text-center">
        <h1>Multi Koder-Dekoder</h1>
      </div>

      <h3>Losowe hasło</h3>
      <input type="text" class="form-control" v-model="input_losowe_haslo"/>
      <label>Typ</label>
      <select v-model="input_losowe_haslo_typ" class="form-control">
        <option value="alfa">Tylko alfanumeryczne</option>
        <option value="all">Wszystkie znaki</option>
      </select>
      <label>Długość hasła</label>
      <input type="number" min="1" v-model="input_losowe_haslo_dlugosc" class="form-control" /><br />
      <input type="button" class="btn btn-primary" value="Losuj" @click="losowe_haslo"/>

      <br /><br />

      <h3>Base64</h3>
      <p>Przyklad kodu: YWxhIG1hIGtvdGE=</p>
      <textarea v-model="input_base64" class="form-control"></textarea><br />
      <div class="btn-group" role="group">
        <input class="btn btn-primary" value="Zakoduj Base64" @click="encode64" />
        <input class="btn btn-secondary" value="Dekoduj Base64" @click="decode64" />
        <input class="btn btn-warning" value="Wyczyść" @click="clearBase64" />
      </div>

      <br /><br />

      <h3>MD5</h3>
      <p>Przyklad kodu: cc0cfe029395b5aa615085fa4e672f09</p>
      <input type="text" v-model="input_md5" class="form-control" /><br />
      <input type="button" class="btn btn-primary" value="Zakoduj MD5" @click="hex_md5" />

      <br /><br />

      <h3>SHA1</h3>
      <p>Przyklad kodu: f0d6dc1e6fd7996fe0c33446a7544bacbf9bf849</p>
      <input type="text" v-model="input_sha1" class="form-control" /><br />
      <input type="button" class="btn btn-primary" value="Zakoduj SHA1" @click="hex_sha1" />

      <br /><br />

      <h3>Kody ASCII</h3>
      <p>Przyklad kodu: 117,110,107,110,111,119</p>
      <input type="text" v-model="input_ascii" class="form-control" /><br />
      <input type="button" class="btn btn-primary" value="Zamień na kody ASCII" @click="toAscii" />

      <br /><br />

      <h3>ROT - przesunięcie dowolne</h3>
      <p>	Przyklad kodu: gb wrfg wnxvf grxfg (rot13)</p>
      <textarea v-model="input_rot" class="form-control"></textarea>
      przesunięcie <input type="number" v-model="input_rot_przesuniecie" class="form-control" /><br />
      <div class="btn-group" role="group">
        <input type="button" class="btn btn-primary" value="Zakoduj" @click="toRot" />
        <input type="button" class="btn btn-secondary" value="Zdekoduj" @click="fromRot" />
      </div>

      <br /><br />

      <h3>URLDECODE</h3>
      <p>Przyklad kodu: %75%77%2D%74%65%61%6D</p>
      <textarea v-model="input_urldecode" class="form-control"></textarea><br />
      <input type="button" class="btn btn-primary" value="Zdekoduj" @click="urldecode" />

      <br /><br />

      <h3>Hex Code</h3>
      <p>Przyklad kodu: 0x55 0x57 0x2d 0x54 0x65 0x61 0x6d 0x2e 0x6f 0x72 0x67</p>
      <textarea v-model="input_hex" class="form-control"></textarea><br />
      <input type="button" class="btn btn-primary" value="Zdekoduj" @click="hexCode" />

      <br /><br />

      <h3>AtBash</h3>
      <p>Przyklad kodu: qzprh gvphg</p>
      <textarea v-model="input_atbash" class="form-control"></textarea><br />
      <input type="button" class="btn btn-primary" value="Koduj / Dekoduj" @click="atbash" />

      <br /><br />

      <h3>XOR - z kluczem</h3>
      <p>Przyklad kodu: lgmou&rcmur (klucz 6)</p>
      <textarea v-model="input_xor" class="form-control"></textarea><br />
      <p>Klucz (liczba lub cyfra!):</p>
      <input type="text" v-model="input_xor_klucz" class="form-control" maxLength="3" /><br />
      <input type="button" class="btn btn-primary" value="Koduj / Dekoduj" @click="xor" />

      <br /><br /><br /><br />
    </div>
  </div>
</template>

<script>
import { encode64, decode64, toAscii, Caesar, hexdecode, atbash, xorek } from '../resources/js/engine';
import { hex_md5 } from '../resources/js/md5';
import { hex_sha1 } from '../resources/js/sha1';

export default {
  name: 'Dekoder',
  data() {
    return {
      input_losowe_haslo: '',
      input_losowe_haslo_typ: 'all',
      input_losowe_haslo_dlugosc: 8,
      input_base64: '',
      input_md5: '',
      input_sha1: '',
      input_ascii: '',
      input_rot: '',
      input_rot_przesuniecie: 13,
      input_urldecode: '',
      input_hex: '',
      input_atbash: '',
      input_xor: '',
      input_xor_klucz: 6
    }
  },
  methods: {
    randomString(length, type='') {
      var chars = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
      if(type=='all'){
        chars += '!@#$%^&*()_+-={}[]:";\'<>?,./|\\';
      }
      var result = '';
      for (var i = length; i > 0; --i) result += chars[Math.floor(Math.random() * chars.length)];
      return result;
    },
    losowe_haslo() {
      this.input_losowe_haslo = this.randomString(this.input_losowe_haslo_dlugosc, this.input_losowe_haslo_typ)
    },
    encode64() {
      this.input_base64 = encode64(this.input_base64)
    },
    decode64() {
      this.input_base64 = decode64(this.input_base64)
    },
    clearBase64() {
      this.input_base64 = ''
    },
    hex_md5() {
      this.input_md5 = hex_md5(this.input_md5)
    },
    hex_sha1() {
      this.input_sha1 = hex_sha1(this.input_sha1)
    },
    toAscii() {
      this.input_ascii = toAscii(this.input_ascii)
    },
    toRot() {
      this.input_rot = Caesar(1,this.input_rot,this.input_rot_przesuniecie)
    },
    fromRot() {
      this.input_rot = Caesar(-1,this.input_rot,this.input_rot_przesuniecie)
    },
    urldecode() {
      this.input_urldecode = decodeURI(this.input_urldecode)
    },
    hexCode() {
      this.input_hex = hexdecode(this.input_hex)
    },
    atbash() {
      this.input_atbash = atbash(this.input_atbash)
    },
    xor() {
      this.input_xor = xorek(this.input_xor, this.input_xor_klucz)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#dekoder{
  background-color: grey;
}
#dekoder .container{
  background-color: white; padding-top: 30px; padding-bottom: 30px;
}
</style>
