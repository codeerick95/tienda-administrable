<template>
    <div class="terminos">
        <section class="product-logo-mobile d-flex justify-content-center align-items-center d-lg-none">
            <img src="/logo.webp" alt="" class="img-fluid w-50 mt-4 mt-lg-0">
        </section>
        <div class="container pt-5">
            <div class="row">
                <article class="col-md-12 px-3 px-lg-5">
                    <h1 class="terminos__title font-weight-bold text-center mb-3">Política de Términos y Condiciones</h1>

                    <p>
                        Bienvenido a <span class="font-weight-bold">{{ nombreTienda }}</span>. Estos términos y condiciones describen las reglas y regulaciones para el uso del sitio web <a :href="url" target="_blank">{{ url }}</a>.
                    </p>

                    <p>
                        <span class="font-weight-bold">{{ nombreTienda }}</span> se encuentra en <a :href="url" target="_blank">{{ url }}</a>.
                    </p>

                    <p>
                        Al acceder a este sitio web, asumimos que aceptas estos términos y condiciones en su totalidad. No continúes usando el sitio web si no aceptas todos los términos y condiciones establecidos en esta página.
                    </p>

                    <p>
                        La siguiente terminología se aplica a estos Términos y Condiciones, Declaración de Privacidad y Aviso legal y cualquiera o todos los Acuerdos: el Cliente, Usted y Su se refieren a usted, la persona que accede a este sitio web y acepta los términos y condiciones de la Compañía. La Compañía, Nosotros mismos, Nosotros y Nuestro, se refiere a nuestra Compañía. Parte, Partes o Nosotros, se refiere en conjunto al Cliente y a nosotros mismos, o al Cliente o a nosotros mismos.
                    </p>

                    <p>
                        Todos los términos se refieren a la oferta, aceptación y consideración del pago necesario para efectuar el proceso de nuestra asistencia al Cliente de la manera más adecuada, ya sea mediante reuniones formales de una duración fija, o por cualquier otro medio, con el propósito expreso de conocer las necesidades del Cliente con respecto a la provisión de los servicios/productos declarados de la Compañía, de acuerdo con y sujeto a la ley vigente de Perú.
                    </p>

                    <p>
                        Cualquier uso de la terminología anterior u otras palabras en singular, plural, mayúsculas y/o, él/ella o ellos, se consideran intercambiables y, por lo tanto, se refieren a lo mismo.
                    </p>

                    <h2 class="lead font-weight-bold">Registro</h2>

                    <p>
                        Para comprar productos en el sitio, es necesario estar registrado. El registro de cada usuario se efectuará completando el formulario interactivo que aparece en el sitio web y enviando dicho formulario, lo cual se realiza automáticamente mediante un "click" en el botón respectivo. La información ingresada por el usuario al formulario se considerará verdadera, cierta e inequívoca, eximiéndonos de cualquier consecuencia generada a causa de información falsa o incorrecta ingresada por el usuario. En la cuenta del Usuario se podrán visualizar los productos favoritos así como el historial de compras.
                    </p>

                    <h2 class="lead font-weight-bold">Cookies</h2>

                    <p>
                        Empleamos el uso de cookies. Al utilizar el sitio web de <span class="font-weight-bold">{{ nombreTienda }}</span>, usted acepta el uso de cookies de acuerdo con la política de privacidad de <span class="font-weight-bold">{{ nombreTienda }}</span>. La mayoría de los modernos sitios web interactivos de hoy en día usan cookies para permitirnos recuperar los detalles del usuario para cada visita.
                    </p>

                    <p>
                        Las cookies se utilizan en algunas áreas de nuestro sitio para habilitar la funcionalidad de esta área y la facilidad de uso para las personas que lo visitan. Algunos de nuestros socios afiliados/publicitarios también pueden usar cookies.
                    </p>

                    <h2 class="lead font-weight-bold">Licencia</h2>

                    <p>
                        A menos que se indique lo contrario, <span class="font-weight-bold">{{ nombreTienda }}</span> y/o sus licenciatarios les pertenecen los derechos de propiedad intelectual de todo el material en <span class="font-weight-bold">{{ nombreTienda }}</span>.
                    </p>

                    <p>
                        Todos los derechos de propiedad intelectual están reservados. Puedes ver y/o imprimir páginas desde <a :href="url" target="_blank">{{ url }}</a> para tu uso personal sujeto a las restricciones establecidas en estos términos y condiciones.
                    </p>

                    <p>
                        No debes:
                    </p>

                    <p>
                        Volver a publicar material desde <a :href="url" target="_blank">{{ url }}</a>.
                    </p>

                    <p>
                        Vender, alquilar u otorgar una sub-licencia de material desde <a :href="url" target="_blank">{{ url }}</a>.
                    </p>

                    <p>
                        Reproducir, duplicar o copiar material desde <a :href="url" target="_blank">{{ url }}</a>.
                    </p>

                    <p>
                        Redistribuir contenido de <span class="font-weight-bold">{{ nombreTienda }}</span>, a menos de que el contenido se haga específicamente para la redistribución.
                    </p>

                    <h2 class="lead font-weight-bold">Control de su información personal</h2>

                    <p>
                        En cualquier momento usted puede restringir la recopilación o el uso de la información personal que es proporcionada a nuestro sitio web.  Cada vez que se le solicite rellenar un formulario, como el de alta de usuario, puede marcar o desmarcar la opción de recibir información por correo electrónico.  En caso de que haya marcado la opción de recibir nuestro boletín o publicidad usted puede cancelarla en cualquier momento.
                    </p>

                    <p>
                        Esta compañía no venderá, cederá ni distribuirá la información personal que es recopilada sin su consentimiento, salvo que sea requerido por un juez con un orden judicial.
                    </p>

                    <h2 class="lead font-weight-bold">Aviso legal</h2>

                    <p>
                        En la medida máxima permitida por la ley aplicable, excluimos todas las representaciones, garantías y condiciones relacionadas con nuestro sitio web y el uso de este sitio web (incluyendo, sin limitación, cualquier garantía implícita por la ley con respecto a la calidad satisfactoria, idoneidad para el propósito y/o el uso de cuidado y habilidad razonables).
                    </p>

                    <p>Nada en este aviso legal:</p>

                    <ul>
                        <li>Limita o excluye nuestra o su responsabilidad por fraude o tergiversación fraudulenta.</li>
                        <li>Limita cualquiera de nuestras o sus responsabilidades de cualquier manera que no esté permitida por la ley aplicable.</li>
                        <li>Excluye cualquiera de nuestras o sus responsabilidades que no pueden ser excluidas bajo la ley aplicable.</li>
                    </ul>

                    <p>Las limitaciones y exclusiones de responsabilidad establecidas en esta Sección y en otras partes de este aviso legal:</p>

                    <ol>
                        <li>están sujetas al párrafo anterior; y</li>
                        <li>rigen todas las responsabilidades que surjan bajo la exención de responsabilidad o en relación con el objeto de esta exención de responsabilidad, incluidas las responsabilidades que surjan en contrato, agravio (incluyendo negligencia) y por incumplimiento del deber legal.</li>
                    </ol>

                    <p>
                        En la medida en que el sitio web y la información y los servicios en el sitio web se proporcionen de forma gratuita, no seremos responsables de ninguna pérdida o daño de ningún tipo.
                    </p>

                    <h2 class="lead font-weight-bold">Acceso y restricciones.</h2>

                    <p>
                        Para acceder al servicio de delivery en el sitio web debe ingresar la dirección en donde el Usuario desea que le llegue su pedido. La dirección del Usuario se asociará a central de producción, y le mostrará los productos que la empresa ofrece en ese momento. Al momento de ingresar la dirección y esta misma sea recibida por la central de acopio, el sitio web podría mostrar restricciones para ser aceptadas por el usuario, y continuar el proceso de compra.
                    </p>

                    <p>
                        Estas restricciones son:
                    </p>

                    <p>
                        5.1 Zona de reparto: El sitio web valida que la dirección ingresada pertenece a la zona de reparto de nuestra empresa. Si la dirección del usuario no correspondiera a una zona de reparto, no se podrá brindar el servicio de delivery solicitado.
                    </p>

                    <p>
                        5.2 Modo de pago: El sitio web indica al usuario los medios de pago existentes: tarjeta de Crédito o Débito por página web, pago en efectivo, con recargo por delivery para cobro, monto que estará indicado en el sitio web.
                    </p>

                    <p>
                        5.3 Tienda cerrada: El sitio web indica al usuario el momento de cierre de toma de pedidos para el siguiente día.
                    </p>

                    <p>
                        5.4 Tiempo de garantía (Horario de entrega): El rango de hora de la entrega del pedido será indicado en el sitio web y puede ser fijado por el cliente al momento de registrar su pedido.
                    </p>

                    <p>
                        5.5 En caso de que la entrega del pedido exceda demasiado tiempo al margen de tolerancia o no se realice la entrega por motivos de fuerza mayor insuperables (accidentes de tránsito del servicio de delivery, desastres naturales, sismos u otros similares) la empresa está exenta de responsabilidad por incumplimiento del servicio; pero, el cliente puede reprogramar su pedido para otra fecha sin costo adicional.
                    </p>

                    <p>
                        5.6 Mayoría de edad: El usuario declara que es mayor de edad y asume responsabilidad sobre los pedidos que realice. En caso no sea mayor de edad la responsabilidad recaerá en sus padres, tutores o curadores.
                    </p>

                    <h2 class="lead font-weight-bold">Libro de reclamaciones</h2>

                    <p>
                        Ponemos a disposición de nuestros Usuarios el Libro de Reclamaciones virtual que figura en nuestra página web.
                    </p>
                </article>
            </div>
        </div>
    </div>
</template>

<script>
import { appConfig } from '@/env'

export default {
    data() {
        return {
            nombreTienda: appConfig.openGraph.titulo,
            url: appConfig.openGraph.urlWeb
        }
    }
}
</script>

<style lang="scss">
.terminos {
    &__title {
        font-size: 1.5em;
    }
}
</style>