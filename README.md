### Hi there, Ramonn Rocha dev 👋

- 📱 React Native
- 🌟 React JS
- 🌆 Front-End

<div align="center">
  <a href="https://github.com/rafaballerini">

<div style="display: inline_block"><br>
  <img align="center" alt="Ramonn-Js" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/javascript/javascript-plain.svg">
  <img align="center" alt="Ramonn-Ts" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/typescript/typescript-plain.svg">
  <img align="center" alt="Ramonn-Node" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/nodejs/nodejs-original.svg">
   <img align="center" alt="Ramonn-React" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original.svg">
  
  <img align="center" alt="Ramonn-HTML" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/html5/html5-original.svg">
  <img align="center" alt="Ramonn-CSS" height="30" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/css3/css3-original.svg">
  <img align="right" alt="Ramonn-pic" height="150" style="border-radius:50px;" src="https://github.com/ramonnrocha/images/blob/main/WhatsApp%20Image%202022-09-30%20at%2015.17.16.jpeg?width=76&height=676">

#


<div> 
  <a href="https://www.instagram.com/ramonnrocha_" target="_blank"><img src="https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white" target="_blank"></a>
 <a href="https://portifolio-beige-nu.vercel.app/" target="_blank"><img src="https://img.shields.io/twitter/url?label=Portif%C3%B3lio&style=for-the-badge&url=https%3A%2F%2Fportifolio-beige-nu.vercel.app%2F" target="_blank"></a> 
  <a href = "mailto:ramonnsantana@gmail.com"><img src="https://img.shields.io/badge/-Gmail-%23333?style=for-the-badge&logo=gmail&logoColor=white" target="_blank"></a>
  <a targer="_blank"href="https://www.linkedin.com/in/ramonn-rocha-santana-049173214" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 
 
  ![Snake animation](https://github.com/rafaballerini/rafaballerini/blob/output/github-contribution-grid-snake.svg)
 
</div>

import { ArrowDownOutlined } from '@ant-design/icons'
import { Box, Button, Card, Collapse, Divider, Grid, Typography } from '@mui/material'
import Column from 'antd/lib/table/Column'
import { format } from 'date-fns'
import { ptBR } from 'date-fns/locale'
import CheckCircleOutline from 'mdi-material-ui/CheckCircleOutline'
import { parseCookies } from 'nookies'
import React, { useEffect, useMemo, useState } from 'react'
import Barcode from 'react-barcode'
import { useQuery } from 'react-query'
import TitleHeader from 'src/@core/components/Title'
import api from 'src/@core/services/api'
import { COLORS } from 'src/@core/utils/theme'


interface Props {
  meiId: string,
}


const CardCNPJ: React.FC<Props> = (props : Props) => {

  const { meiId } = props;

  const [open, setOpen] = React.useState(false)
  const [itemExpanded, setItemExpanded] = React.useState<boolean | null>(false)
  const [ show, setShow ] = useState(null)
  const [ loading ,setLoading] = useState(false)
  const [userData ,setUserData] = useState(null)
  const [ loadingInitial ,setLoadingInitial] = useState(false)
  const [datas, setDatas] = useState(new Date())
  const dateFormatted = useMemo(() => format(datas, 'yyyy', { locale: ptBR }), [datas])

  async function getDadosAsync() {
    try {
      setLoadingInitial(true);
      const { data } = await api.post('/cnpj/list', {
        cnpj: cnpj_user.replace(/\D+/g, ''),
      });

      if (data?.base64 && data?.createdAt) {
        setCodigoAsync(data);
      }
    } catch (error) {
    } finally {
      setLoadingInitial(false);
    }
  }

  useEffect(() => {
    getDadosAsync();
  }, []);

  async function getDate() {
    setTimeout(() => {
      setLoading(true);
      setShow(true);
    }, 100);

    try {
      const response = await api.post('/cnpj/v1', {
        cnpj: cnpj_user.replace(/\D+/g, ''),
      });

      if (response.data?.status === 'IN_PROGRESSO') {
        setShow(false);
        setTimeout(() => {
          setModalSucesso(true);
        }, 500);
      }

      if (response?.data?.base64) {
        setCodigo(response.data.base64);
      }
    } catch (err) {
      setShow(false);
      setTimeout(() => {
        Alert.alert(
          'Atenção',
          'Houve um error ao gerar o cartão CNPJ. Tente novamente em instantes.',
        );
      }, 10);
    } finally {
      setLoading(false);
    }
  }


  function onChangePayments() {
    setOpen(false)
    setItemExpanded(null)
  }

  return (
    <>
      <TitleHeader title={'Cartão CNPJ'} optionsBack={true} />
      <div style={{ padding: 20,}} />
      <Grid container flexDirection="column" alignItems='center' justifyContent='center' sx={{ width: "100%" }}>
      <Grid
        item
        xs={12}
        md={12}
        lg={7}
        gap={10}
        style={{
          height: '100vh',
          alignItems: 'center',
          display: 'flex',
          flexDirection: 'column',
          justifyContent: 'flex-start'
        }}
        >
          <Typography
            variant='body1'
            sx={{
              justifyContent: "flex-start",
              fontSize: 24,

            }}
          >
            Emissão de Comprovante (CNPJ)
          </Typography>
          <Typography >
            Aqui você pode visualizar, compartilhar o seu cartão CNPJ em um único lugar.
          </Typography>
          <Box sx={{ mb: 8, display: 'flex', alignItems: 'center', justifyContent: 'center' }}>
            <img
              src='https://i.ibb.co/KrGdvJf/cartao-CNPJ.png'
              alt="image"
              style={{
                width: "20rem",
                height: "auto"
              }}
            />
          </Box>
          <Button  size='large' type='submit' variant='contained' sx={{ marginBottom: 7, borderRadius: 8 }}>
                Emitir agora mesmo
          </Button>
        </Grid>
            <Typography sx={{ mt: 10, mb: 10 }}>
              Último cartão gerado :
            </Typography>

      </Grid>
      <Grid sx={{marginBottom: 10}}>
          <Divider sx={{mb: 10}}/>
          <Button
            fullWidth
            onClick={onChangePayments}
            sx={{
              flexDirection: 'row',
              justifyContent: 'space-between',
              alignItems: 'center',
              display: 'flex',
              padding: 3,
              color: COLORS.cinzaServemei,
              '&:hover': {
                boxShadow: 'none',
                backgroundColor: '#99999911'
              }
            }}
          >
              <Grid sx={{ display: "flex", flexDirection: "column"}}>
                  <Typography>
                    nomedapessoa.pdf
                  </Typography>
                  <span style={{fontSize: 12, }}>Gerado 02/07/2024 </span>
              </Grid>
            <ArrowDownOutlined />
          </Button>

      </Grid>
    </>
  )
}

export default CardCNPJ

export const getServerSideProps = async (ctx: any) => {
  const { ['servemei_app_auth.token']: token } = parseCookies(ctx)
  const { ['servemei_app_auth.meiId']: meiId } = parseCookies(ctx)

  if (!token) {
    return {
      redirect: {
        destination: '/pages/login/',
        permanent: false
      }
    }
  }

  return {
    props: { meiId }
  }
}
