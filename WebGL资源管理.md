1. ����GC��˵ĵط�
������֪��JS��ӵ��GC�����ԡ�ȫ��JS�������㲻��Ҫ��ʱ���ᱻJS�������Զ��ػ��յ���������Ϊ���ǹ�����Դ����ʲôҪ�Լ������֣�
������������Ȼ���һ��������������WebGL����ɡ�
var tex = gl.createTexture()
gl.bindTexture(gl.TEXTURE_2D, tex)
gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGB, gl.RGB, gl.UNSIGNED_BYTE, image)
gl.bindTexture(gl.TEXTURE_2D, null)
���������������
gl.deleteTexture(tex)
�������ϤOpenGL��OpenGLESһ�����ö���δ��롣gl.createTexture�����������أ�gl.bindTexture������Ҫ����������gl.deleteTexture������������
�ȵȡ�������������JS����GC��˵��ΪʲôҪ�ֶ���������������Ϊ�˺�OpenGL����һ�£������Ǳ��ԭ��API����Ƴ���������Դ�����������ǵ��������뵽WebGL/OpenGL��buffer��shader��framebuffer��texture�����ƵĲ������̡���
�Բۣ�WebGL�������ƿ���JS���������Щ�������Դ��еĶ����ǲ���JS�����GC��Ͻ�ġ�������������ǣ�Ҳ���ǡ�
var canvas = document.createElement("canvas")
var gl = canvas.getContext("webgl")
// do any thing you like.
gl = null
canvas = null
�������ǳ�ʼ��webGL�Ĵ��룬webGL�Ǵ�һ��canvas�ϴ����ģ�������ر�д����gl��canvas�ÿյĴ��룬��Ϊ����Ҫ��JS������������ǽ�webGL���õ���ȫ����Դ����������canvas������������������canvasʱ��WebGL�ϵ�ȫ����Դ��canvasһ��ȴ���GC���١�
����WebGL����JS�����ֱϽ�µ��ر���������
�ʣ�����JS��û���������
���С�
�ʣ���ô���texture���ǹܲ��ܣ�
�𣺲��ܡ�
��#@~:%@&=$+������δ������˺����޳�֮�������롣
�Դ�����ܻ������ʣ���OpenGL��gl.createTexture���ص���һ���������ͣ���ָ�룩��һ������ľ��������texture�ȶ���һ����OpenGL����ʱ��������webGL���ǡ�
console.log(textrue) // => WebGLTexture {}
��chrome�д�ӡһ��textrue������textrue�Ǹ�����WebGLTexture��ԭ�����󡣚G~�Ƕ��󣬶�����Ա�GCѽ��˵����gl.deleteTexture�����Ǹ�JS�������һ����ʾ��hint��������JS��������̻�������������JS��GCʵ�֣�mark and sweep����һ�����ڣ��������̻��շ������Դ����gl.deleteTexture�������ֲ����ȱ��ģ�������������õ�GC�ˣ�����ɾ������������˵���㡣
perfect��������Ϳ������������ź���һ���Ľ�����Google��һ�´��ǡ����񶨵ġ�����Ȥ��ͬѧ���Բμ���������
����JS��û�����������ġ���û��C++��~Class()��������ʱ�ض����õĺ�����Ҳ��ûJava��finalize()��Python��__del__()������������Զ����õ���Դ�ͷź�����texture�ͷ��ɳ���Ա�Լ���֤��һҹ�����ǻص���������70���C�������¡�
���ڻ��ǿ�������һ�£���������������һЩ������


2. ����һ���Ӷ�����
�������WebGLֻ������չʾ����3Dģ�ͣ�һ�鲻��Ĺ̶�����������һ����ȵ����Σ���ô�Դ��Ӷ������ɡ�
��Ϊ����2016�꣩�������Ժʹ󲿷��ֻ����ṩ300M-500M���Դ棨�ڴ棩�������⡣��3DӦ���У�����Դά�����Դ棨�ڴ棩�ﻹ������Ӧ�õ������ȡ����㲻��Ҫ<cnavas>ʱ��JSһ��������յ������ֶ���Ϊ����
�����������Ҫ��ʱ�����У����ϼ�������Դ��Ӧ���С�����AVG��Ϸ�У����ž����ƽ����ϳ��ֵı����������CG�����������������ٱ���һ��ӵ�о޴��ͼ��RPG��


3. ��������A hack way
�����ҽ�һ��ʱ���ڿ����������������������Ȼ����һЩ�����ԣ���֮��˵��������Ȼֵ��ѧϰ������������ԭ�������ԡ�
����һ�����ǵ�Ŀ�ģ�����WebGL�е���������WebGLTexture����ʲô��˼������һֱĬ��WebGLTexture���������������WebGLTextureҲ���Բ���Ӧ������gl.texImage2D()����֮ǰ��
gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGB, gl.RGB, gl.UNSIGNED_BYTE, image)
���������image��ʲô�������HTML�е�<image>Ҳ������<canvas><video>�ȡ�
OK��WebGLTexture JS��������ܣ�<image>�ܵùܰɡ����Կ���<image>�������¡�
var texArray = [
    gl.createTexture(),
    gl.createTexture(),
    gl.createTexture(),
    gl.createTexture(),
]
//  0 <= index <= 3
function bindImage(index, image) {
    gl.bindTexture(gl.TEXTURE_2D, tex[index])
    gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGB, gl.RGB, gl.UNSIGNED_BYTE, image)
    gl.activeTexture(_webGL.TEXTURE0 + index)
    gl.bindTexture(gl.TEXTURE_2D, null)
}
ʹ�������ʱ�򲻴���WebGLTexture����֮��
bindImage(0, image0)
bindImage(1, image1)
// ...
gl.drawArray()
��ʼ�����㹻��WebGLTexture��������4����Ҳ���Ը��࣬������Ҫ����Ҫֱ�ӽ�<image>�󶨵�WebGLTexture���ǵ��õ�ʱ�ٰ󶨡�Ȼ����ᷢ�֣����ⲻ�����ˣ����˵�ǰ���󶨵�<image>������<image>�Ѿ����Ա�JS��GCʶ������
��������˵������������һ���ù�OpenGL�����Żỳ�����������Ч�����⡣OpenGL��gl.texImage2D()������ʵ��ʵ�ؽ�image�е��������ڴ洫�䵽�Դ档��������������²�һ�����ִ������Ϊ��Ч�ʻ����GPU����Ⱦҳ�棬�᲻��<image>�������һ�������Ѿ����������Դ������ء��᲻��gl.texImage2D()����ʵ���ϲ�û�д������ݣ�����ȡ����<image>�б������ڵ�OpenGL����ء�
���ϴ����²⣬�������������ʵ�֣���һ�´����ǿ����ˡ�����һ��gl.texImage2D()�е����һ����������<image>������<canvas>��<video>��������פ�Դ���ûʲô�����˰ɡ�<canvas>��2dAPI����һ������drawImage()�ĺ��������ǿ��԰�<image>����<canvas>�ϣ���<canvas>����<image>��
���ڸ÷����ľ����ԣ�ֻ������texture�Ĺ���������<canvas>����framebuffer������shader��bufferû������
����2D��Ϸ��cocos2d����egretʲô�ģ���֪����2d��Ϸ�в����ڸ��ӵ�ģ�ͣ�����������ƶ�����ӳ��������ľ��Σ�����֮Ϊsprite��spriteͨ����û�ж�Ӧbuffer�ģ���ͼʱ�����潫sprite�Ķ�������д�뵽һ�����õ�buffer�У���������ύGPU���ơ����ε�������С������buffer������һЩ�ر���Ż�����Ҫ���������ƣ�����WebGL��draw call ֮��ģ���buffer������Ҳ���Խ����
����shader��ȫ����פ�ڴ��ֺη��أ�
�������������2D��Ϸ����ȫ�ǿ��õģ�AVG�������𣿿��ԡ�
�š����������ҵ������������
��Ϊһ����׷���ɧ�꣬�һ�����3D�ı�����������ϵͳ��Live2d���͵����񶯻���������3��2�Ŀ�ͨ��Ⱦ������һ�ԡ���


4. ����C++
֮ǰ�����ۣ�����һֱ������JS�ķ�����Ѱ�ҽ���취�����ǵ�������ʵ�Ǹ�����GC������Դ����GC���������Ǹ�ԭʼ�����ԣ�����C++������������������η�Ӧ���Լ�ܳһ����������ѡ���ü�������Ϊ���򵥣��򵥣��򵥡�
������ù�C++�������� C++ primer ��������ü�������Ϥ��Ϊ�˾��������ֶ�IncRef()��DecRef()������������ƣ�
function Textrue(url) {
    this.glTexture = gl.createTexture()
    // init texture ...
}
Texture.IncRef = function() { /* ... */ }
Texture.DecRef = function() { /* ... */ }
var textureMap = {}
funciton newTexture(name) {
    if (textureMap[name]) {
        return textureMap[name]
    } else {
        var texture = new Texture(name)
        textureMap[name] = texture
        return texture;
    }
}
Ϊ�˷����ʹ��Texture�࣬���ǵ���ʾ����Ҳ��Ҫ���졣
function Image() {
    this.texture = null;
    // ...
}
Image.prototype.setTexture(texture) {
    texture.IncRef()
    if (this.texture && 0 == this.texture.DecRef()) {
        tex.destory();
    }
}
���ǽ�������ȫ��texture������һ��map�������ü��������map������Դ�����ͬʱ���е��˻�������񣬵��㴴������url��ͬ��textureʱ��������Ҫ�½�һ��texture��
var image = new Image()
var texture = newTexture(url)
image.setTexture(texture)
��Texture����Image֮��Image���ں��ʵ�ʱ������Texture��
˵ʵ�����ü�����ǿ���ã�ֻ������Image֮��������Texture��������е����ˣ�����Լ��������ü��������ߴ�����ArrayRef��MapRef֮�����������������С��ѭ�����õ����⣬����һ�������Texture����������һ��Tex�ɡ�
����ԭ��API�н���������Ȼԭʼ���ɰ汾��FenQi.Engineʹ�õ������ü�����

5. 













